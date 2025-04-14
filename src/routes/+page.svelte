<script lang="ts">
	const chat = $state({
		messages: [] as { sender: string; text: string }[],
		input: '',
		isLoading: false
	});

	// Handle form submission
	async function handleSubmit() {
		// Add user message to the UI
		chat.messages = [...chat.messages, { sender: 'user', text: chat.input }];
		chat.input = ''; // Clear input
		chat.isLoading = true;

		try {
			// Send message to n8n
			const response = await fetch('http://localhost:5678/webhook-test/chatbot', {
				method: 'POST',
				headers: { 'Content-Type': 'application/json' },
				body: JSON.stringify({ message: chat.input })
			});

			if (!response.ok) {
				throw new Error(`HTTP error! Status: ${response.status}`);
			}

			const data = await response.json();
			console.log('Parsed JSON:', data);

			// Add bot response to the UI (adjust based on n8n response structure)
			chat.messages = [...chat.messages, { sender: 'bot', text: data.output || 'No response' }];
		} catch (error) {
			console.error('Error occurred:', error);
		} finally {
			chat.isLoading = false;
		}
	}
</script>

<layout>
	<div class="max-w-4xl px-4 py-10 sm:px-6 lg:px-8 lg:py-14 mx-auto">
		<ul class="mt-16 space-y-5">
			<!-- Chat Bubble -->
			<li class="flex gap-x-2 sm:gap-x-4">
				<span
					class="shrink-0 inline-flex items-center justify-center size-9.5 rounded-full bg-blue-900"
				>
					<span class="text-sm font-medium text-white">Bot</span>
				</span>

				<!-- Card -->
				<div
					class="inline-block bg-white border border-gray-200 rounded-lg p-4 space-y-3 dark:bg-neutral-900 dark:border-neutral-700"
				>
					<h2 class="font-medium text-gray-800 dark:text-white">Hi! 👋 I'm an AI Agent.</h2>
					<div class="space-y-1.5">
						<p class="mb-1.5 text-sm text-gray-800 dark:text-white">
							I'll help you get started. Ready? 🚀
						</p>
					</div>
				</div>
				<!-- End Card -->
			</li>
			<!-- End Chat Bubble -->

			{#each chat.messages as { sender, text }}
				<!-- User Chat Bubble -->
				{#if sender === 'user'}
					<li class="max-w-2xl ms-auto flex justify-end gap-x-2 sm:gap-x-4">
						<div class="grow text-end space-y-3">
							<!-- Card -->
							<div class="inline-block bg-blue-600 rounded-lg p-4 shadow-2xs">
								<p class="text-sm text-white">
									{text}
								</p>
							</div>
							<!-- End Card -->
						</div>

						<span
							class="shrink-0 inline-flex items-center justify-center size-9.5 rounded-full bg-blue-600"
						>
							<span class="text-sm font-medium text-white">You</span>
						</span>
					</li>
					<!-- End Chat Bubble -->
				{:else if sender === 'bot'}
					<!-- Chat Bubble -->
					<li class="flex gap-x-2 sm:gap-x-4 {sender}">
						<span
							class="shrink-0 inline-flex items-center justify-center size-9.5 rounded-full bg-blue-900"
						>
							<span class="text-sm font-medium text-white">Bot</span>
						</span>

						<!-- Card -->
						<div
							class="inline-block bg-white border border-gray-200 rounded-lg p-4 space-y-3 dark:bg-neutral-900 dark:border-neutral-700"
						>
							<div class="space-y-1.5">
								<p class="mb-1.5 text-sm text-gray-800 dark:text-white">
									{text}
								</p>
							</div>
						</div>
						<!-- End Card -->
					</li>
					<!-- End Chat Bubble -->
				{/if}
			{/each}
		</ul>
	</div>

	<form
		onsubmit={(e) => {
			e.preventDefault();
			handleSubmit();
		}}
	>
		<div
			class="w-full max-w-4xl mx-auto sticky bottom-0 z-10 bg-white border-t border-gray-200 px-4 sm:px-6 lg:px-0 dark:bg-neutral-900 dark:border-neutral-700"
		>
			<div class="mt-10 max-w-2xl w-full mx-auto px-4 sm:px-6 lg:px-8">
				<div class="relative">
					<input
						type="text"
						bind:value={chat.input}
						class="p-3 sm:p-4 block w-full border border-gray-200 rounded-full sm:text-sm focus:border-blue-500 focus:ring-blue-500 disabled:opacity-50 disabled:pointer-events-none dark:bg-neutral-900 dark:border-neutral-700 dark:text-neutral-400 dark:placeholder-neutral-500 dark:focus:ring-neutral-600"
						placeholder="Ask me anything..."
						disabled={chat.isLoading}
					/>
					<div class="absolute top-1/2 end-2 -translate-y-1/2">
						<button
							type="submit"
							aria-label="Send message"
							disabled={!chat.input.trim() || chat.isLoading}
							class="size-10 inline-flex justify-center items-center gap-x-2 text-sm font-semibold rounded-full border border-transparent text-gray-500 hover:text-gray-800 focus:outline-hidden focus:text-gray-800 bg-gray-100 disabled:opacity-50 disabled:pointer-events-none dark:text-neutral-400 dark:bg-neutral-800 dark:hover:text-white dark:focus:text-white"
						>
							<svg
								xmlns="http://www.w3.org/2000/svg"
								width="24"
								height="24"
								viewBox="0 0 24 24"
								fill="none"
								stroke="currentColor"
								stroke-width="2"
								stroke-linecap="round"
								stroke-linejoin="round"
								class="lucide lucide-send-icon lucide-send"
								><path
									d="M14.536 21.686a.5.5 0 0 0 .937-.024l6.5-19a.496.496 0 0 0-.635-.635l-19 6.5a.5.5 0 0 0-.024.937l7.93 3.18a2 2 0 0 1 1.112 1.11z"
								></path><path d="m21.854 2.147-10.94 10.939"></path></svg
							>
						</button>
					</div>
				</div>
			</div>
		</div>
	</form>
</layout>
