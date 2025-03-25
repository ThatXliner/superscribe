<script lang="ts">
	import { Button } from '$lib/components/ui/button';
	export const { onAudioChunk, onFinishRecord, chunkSize } = $props<{
		onAudioChunk: (x: Blob) => void;
		onFinishRecord: (x: Blob) => void;
		chunkSize?: number;
	}>();
	let mediaRecorder = $state<MediaRecorder>();
	let chunks: Blob[] = $state([]);
	async function initialize() {
		if (navigator.mediaDevices?.getUserMedia) {
			console.log('getUserMedia supported.');
			const stream = await navigator.mediaDevices.getUserMedia(
				// constraints - only audio needed for this app
				{
					audio: true
				}
			);
			mediaRecorder = new MediaRecorder(stream);

			mediaRecorder.ondataavailable = (e) => {
				chunks.push(e.data);
				onAudioChunk(e.data);
			};
			mediaRecorder.onstop = (e) => {
				console.log('Chunk type:', chunks[0].type);
				const blob = new Blob(chunks, { type: chunks[0].type });
				onFinishRecord(blob);
				chunks = [];
				isRecording = false;
			};
		} else {
			throw new Error('getUserMedia not supported on your browser!');
		}
	}
	let isRecording = $state(false);
</script>

<Button
	onclick={async () => {
		if (mediaRecorder === undefined) {
			await initialize();
		}
		// Should be impossible now. Only here for TypeScript
		if (mediaRecorder === undefined) {
			return;
		}
		if (isRecording) {
			mediaRecorder.stop();
		} else {
			mediaRecorder.start(chunkSize ?? 5000);
			isRecording = true;
		}
	}}>{isRecording ? 'Stop' : 'Record'}</Button
>
