# EXIF Tag Viewer

A SvelteKit 5 web app for viewing EXIF metadata from image files, with full offline support and PWA features. Built with Svelte, Tailwind CSS, and exifr.

## Features

- Drag-and-drop or select images (JPG, JPEG, HEIC, HEIF)
- Displays EXIF metadata in a responsive table
- Shows image thumbnails
- Works offline as a PWA (installable on iOS/Android)
- Mobile-friendly UI with Tailwind CSS

## Getting Started

### Development

Install dependencies:

```bash
bun install
```

Start the development server:

```bash
bun run dev
```

Visit [localhost:5173](http://localhost:5173) (or the port shown in your terminal).

### Building for Production

Build a static version of the app:

```bash
bun run build
```

The static site will be output to the `build` directory.

### Serving the Static Site

You can serve the static build locally with:

```bash
bun add -g serve
serve build
```

### PWA & Offline Support

- The app is installable as a PWA and works offline.
- Add to your iOS/Android home screen for a native-like experience.

## Tech Stack

- [SvelteKit 5](https://kit.svelte.dev/)
- [Tailwind CSS](https://tailwindcss.com/)
- [exifr](https://github.com/MikeKovarik/exifr) (for EXIF parsing)
- [Vite PWA Plugin](https://vite-pwa-org.netlify.app/)

## License

MIT
