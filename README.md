# Chatbot for Websites

A Svelte-based chatbot component designed to integrate seamlessly into your website. This project is powered by [`SvelteKit`](https://kit.svelte.dev/) and uses [`TailwindCSS`](https://tailwindcss.com/) for styling.

## Features

- **Interactive Chatbot**: A ready-to-use chatbot component that can be easily embedded into your website.
- **Customizable**: Modify the chatbot's appearance and behavior to suit your needs.
- **API Integration**: Communicates with an external API (e.g., `n8n`) to handle chatbot responses.
- **Dark Mode Support**: Built-in support for light and dark themes.

## Getting Started

### Prerequisites

Ensure you have the following installed:

- [Node.js](https://nodejs.org/) (v16 or higher)
- [npm](https://www.npmjs.com/) or [pnpm](https://pnpm.io/) or [yarn](https://yarnpkg.com/)

### Installation

Clone the repository and install dependencies:

```bash
git clone https://github.com/pblux/chatbot-for-websites.git
cd chatbot-for-websites
npm install
```

### Development

Start the development server:

```bash
npm run dev
```

This will start a local development server. Open [http://localhost:5173](http://localhost:5173) in your browser to view the app.

### Building for Production

To create a production build of your app:

```bash
npm run build
```

Preview the production build locally:

```bash
npm run preview
```

### Linting and Formatting

- **Linting**: Run ESLint to check for code issues:

  ```bash
  npm run lint
  ```

- **Formatting**: Use Prettier to format your code:

  ```bash
  npm run format
  ```

### Testing

Run type checking and other static analysis:

```bash
npm run check
```

## Usage

### Embedding the Chatbot

To use the chatbot in your project, import it and include it in your Svelte component:

```svelte
<script lang="ts">
	import Chatbot from '$lib/Chatbot.svelte';
</script>

<Chatbot />
```

### API Integration

The chatbot sends user messages to an external API (e.g., `n8n`) and displays the response. Update the API endpoint in [`Chatbot.svelte`](src/lib/Chatbot.svelte) to match your backend:

```ts
const response = await fetch('http://localhost:5678/webhook/chatbot', {
	method: 'POST',
	headers: { 'Content-Type': 'application/json' },
	body: JSON.stringify({ message: chat.input })
});
```

## Project Structure

```
├── src/
│   ├── lib/
│   │   └── Chatbot.svelte  # Chatbot component
│   ├── routes/
│   │   └── +page.svelte    # Main page
│   ├── app.css             # Global styles
│   └── app.html            # HTML template
├── .svelte-kit/            # Generated files
├── static/                 # Static assets
├── package.json            # Project metadata and scripts
└── README.md               # Project documentation
```

## Configuration

### TailwindCSS

TailwindCSS is preconfigured in this project. To customize styles, edit the `tailwind.config.js` file or add custom CSS in `src/app.css`.

### Environment Variables

Environment variables can be configured in `.env` files. For example:

```env
PUBLIC_API_URL=https://your-api-url.com
```

## Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository.
2. Create a new branch: `git checkout -b feature-name`.
3. Commit your changes: `git commit -m "Add feature"`.
4. Push to the branch: `git push origin feature-name`.
5. Open a pull request.

## License

This project is licensed under the [MIT License](LICENSE).

## Acknowledgments

- [SvelteKit](https://kit.svelte.dev/)
- [TailwindCSS](https://tailwindcss.com/)
- [n8n](https://n8n.io/)

---

Happy coding!
