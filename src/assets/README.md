# vue-windows-explorer

A Vue 3 project inspired by Windows Explorer, designed to manage and display hierarchical data in a user-friendly interface. Built with Vuetify, Vite, and modern JavaScript tooling.

## Features

- **Modern UI**: Built with Vuetify for responsive and accessible design.
- **Hierarchical Data Management**: Intuitive navigation for tree-like structures.
- **Fast and Efficient**: Powered by Vite for lightning-fast development and builds.
- **State Management**: Pinia for managing application state.
- **Linting**: ESLint integration for clean and consistent code.
- **Axios for API calls**: Simplifies HTTP requests.

## Project Setup

### Prerequisites

- Node.js >= 16
- npm or Bun

### Installation

1. Clone the repository:

   ```bash
   git clone [text](https://github.com/rizkiwhy/vue-windows-explorer.git)
   cd vue-windows-explorer
   ```

2. Install dependencies:
   Using npm:
   ```bash
   npm install
   ```
   Or using Bun:
   ```bash
   bun install
   ```

### Development

Start the development server:

```bash
npm run dev
```

Or with Bun:

```bash
bun dev
```

The app will be available at `http://localhost:5173`.

### Build

To build the project for production:

```bash
npm run build
```

The built files will be in the `dist/` directory.

### Preview

To preview the production build:

```bash
npm run preview
```

### Linting

To lint and fix code issues:

```bash
npm run lint
```

## Technologies Used

- **Vue 3**: Core framework for building the application.
- **Vuetify**: Material Design Component Framework.
- **Vite**: Next-generation frontend tooling for fast builds and HMR.
- **Pinia**: Store library for Vue.
- **Vue Router**: Client-side routing.
- **Sass**: CSS preprocessor for styling.
- **Axios**: Promise-based HTTP client.
- **Unplugin Libraries**: Auto-imports for components and fonts.

## Folder Structure

- `src/`: Application source code.
  - `components/`: Reusable Vue components.
  - `layouts/`: Layout templates.
  - `router/`: Vue Router configuration.
  - `store/`: Pinia store definitions.
  - `views/`: Page views.
- `public/`: Static assets.
- `dist/`: Built files (generated after running `npm run build`).

## Dependencies

**Main Dependencies:**

- Vue 3
- Vuetify
- Vue Router
- Pinia
- Axios

**Development Dependencies:**

- Vite
- ESLint
- Sass (Dart Sass & Sass Embedded)
- Unplugin Libraries for automation
- Plugins for Vue and Vuetify

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.

## Contributing

Contributions are welcome! Feel free to submit issues and pull requests.

### Steps to Contribute

1. Fork the repository.
2. Create a new branch:
   ```bash
   git checkout -b feature/your-feature
   ```
3. Make your changes and commit them:
   ```bash
   git commit -m "Add your feature"
   ```
4. Push to the branch:
   ```bash
   git push origin feature/your-feature
   ```
5. Open a pull request on GitHub.

---

Thank you for checking out **vue-windows-explorer**! If you have any questions, feel free to reach out.
