# Dynamic Placeholder Replacer

A client-side web application that dynamically generates forms from text containing placeholders in `<placeholder>` format and replaces them with user input. All processing happens in the browser with no server communication.

## Features

- Real-time placeholder detection and highlighting
- Dynamic form generation
- Instant preview with syntax highlighting
- One-click copy to clipboard
- Fully client-side processing (no data sent to server)

## Docker Setup

### Build the Docker image

```bash
docker build -t placeholder-replacer .
```

### Run the container

```bash
docker run -p 8080:80 placeholder-replacer

docker run -d -p 8080:80 placeholder-replacer
```

```bash
docker ps | grep placeholder-replacer
```

The application will be available at `http://localhost:8080`

## Usage

1. Enter text with placeholders like `Hello <name>, welcome to <website>!`
2. Fill out the dynamically generated form
3. Copy the final output with replaced placeholders

## Files

- `index.html` - Main HTML structure
- `style.css` - Styling and design system
- `app.js` - JavaScript logic for placeholder processing
- `Dockerfile` - Container configuration