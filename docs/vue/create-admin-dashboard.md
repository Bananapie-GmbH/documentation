# Dashboard Template for SPA in Nuxt3

To use the template please open: https://github.com/Bananapie-GmbH/dashboard-template

A modern dashboard template built with Vue 3, Vuetify, and Nuxt 3. This template provides a solid foundation for building admin dashboards and data-driven applications with authentication, internationalization, and a clean component architecture.

## ✨ Features

- **Vue 3 + Nuxt 3** - Modern framework with server-side rendering
- **Vuetify** - Material Design component framework
- **Authentication** - Built-in auth middleware and user management
- **Internationalization** - Multi-language support (English, German)
- **API Integration** - Organized API clients and services
- **State Management** - Pinia stores for application state
- **Responsive Design** - Mobile-first approach with Vuetify components
- **TypeScript** - Full TypeScript support for better development experience

## 🚀 Quick Start

### Prerequisites

- Node.js (22.x or later)
- npm, pnpm, yarn, or bun

### Installation

```bash
# Clone the repository
git clone <your-repo-url>
cd dashboard-template

# Install dependencies
npm install
# or
pnpm install
# or
yarn install
```

### Development

Start the development server:

```bash
npm run dev
# or
pnpm dev
# or
yarn dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

### Production

Build for production:

```bash
npm run build
# or
pnpm build
# or
yarn build
```

Preview production build:

```bash
npm run preview
# or
pnpm preview
# or
yarn preview
```

## 📁 Project Structure

```
├── api/                 # API clients, services, and interfaces
├── components/          # Reusable Vue components
├── composables/         # Vue composables for shared logic
├── i18n/               # Internationalization files
├── layouts/            # Nuxt layouts (default, login)
├── middleware/         # Route middleware (auth, etc.)
├── pages/              # Application pages/routes
├── plugins/            # Nuxt plugins
├── stores/             # Pinia stores for state management
└── public/             # Static assets
```

## 🛠️ Customization

This template is designed to be easily customizable:

- **Theming**: Modify Vuetify theme in `plugins/vuetify.ts`
- **Authentication**: Update auth logic in `middleware/auth.ts`
- **API**: Extend API services in the `api/` directory
- **Components**: Add custom components in `components/`
- **Translations**: Add new languages in `i18n/locales/`

## 📚 Documentation

- [Nuxt 3 Documentation](https://nuxt.com/docs)
- [Vue 3 Documentation](https://vuejs.org/guide/)
- [Vuetify Documentation](https://vuetifyjs.com/)

## 🤝 Contributing

Feel free to submit issues and pull requests to improve this template.

## 📄 License

This project is open source and available under the [MIT License](LICENSE).
