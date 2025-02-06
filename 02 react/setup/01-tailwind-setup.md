# Setting up Tailwind CSS in a Vite-React project

NOTE: This guide assumes you have a Vite-React project set up. If you don't, you can follow the guide [here](https://vitejs.dev/guide/).

Visit the [Tailwind CSS Vite installation guide](https://tailwindcss.com/docs/guides/vite) for the most up-to-date instructions.

We are installing Tailwind CSS version `v3.4.13` in this guide.

**1. Install Tailwind CSS and its dependencies:**

Install `tailwindcss` and its peer dependencies.

```bash
npm install -D tailwindcss postcss autoprefixer
```

Then generate your tailwind.config.js and `postcss.config.js` files.

```bash
npx tailwindcss init -p
```

**2. Configure your template paths:**

Add the paths to all of your template files in your `tailwind.config.js` file.

```javascript
/** @type {import('tailwindcss').Config} */
export default {
  content: ["./index.html", "./src/**/*.{js,ts,jsx,tsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

**3. Add the Tailwind directives to your CSS:**

Add the @tailwind directives for each of Tailwind’s layers to your `./src/index.css` file.

Add on top of the file:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

---

## Enable sorting of tailwindcss classes in VSCode

Install Prettier and Tailwind CSS IntelliSense extensions in VSCode.

1. Install Prettier.

```bash
npm install -D prettier prettier-plugin-tailwindcss
```

2. Create a `.prettierrc` file in the root of your project and add the following configuration.

```json
{
  "plugins": ["prettier-plugin-tailwindcss"]
}
```
