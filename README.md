# `ffs`

## Create new project

- `dfx new ffs`
- `cd ffs`

## Deploy the project

- `dfx build` will not work at this point: `Error: Cannot find canister id. Please issue 'dfx canister create ffs_backend'.`
- `dfx deploy` will work: `dfx deploy` is just a shorthand for `dfx canister crate`, `dfx build`, `dfx canister install`.

## Install shadcn

- Following shadcn/Vite - Instaling Tailwind in src/frontend

```
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init
```

package.json is updated: tailwindcss, postcss, autoprefixer. We have a new `tailwind.config.js`

## Tweak tsconfig.json

Here the problem starts cause the scaffold we get from dfx is different from the one we get from `npm install vite@latest` which is the one supposed by schadcn. The scaffold one has two files `tsconfig.app.json` and `tsconfig.node.json` which are not present in the dfx scaffold. The `vite.config.js` in the scaffold of vite-latest is a ts file. So we converted the file to ts, we had to add node types for the `url` module in our dfx config, and we had to tweak the tsconfig.app.json and the node one allowing composite and disabling noEmit.

### Why We Need to Change These TypeScript Values:

- **`composite: true`**: When using **project references**, TypeScript requires the referenced `tsconfig` files to have `"composite": true`. This enables **incremental builds**, ensuring that TypeScript can properly track dependencies between projects and compile them in the correct order. Without this setting, the TypeScript compiler will raise errors when trying to reference projects.

- **`noEmit: false`**: By default, TypeScript project references expect referenced projects to be able to emit files (such as `.js` or `.d.ts`). If `"noEmit": true` is set, TypeScript will not emit these files, which can break the reference chain. Setting `"noEmit": false` or removing it allows the necessary output files to be generated, even if you're not directly using them in your project (e.g., for tooling or type-checking purposes).

This provides the necessary context for why these settings are required in your project when using TypeScript project references.kk

### Summary for Documentation

Here's a concise summary for your documentation explaining the additions to the `tsconfig.json` file and their purposes:

---

### **Additions to `tsconfig.json`**

1. **`files` and `references`**:

   - **`files`:** An empty array indicates that no specific files are included by default. It's a placeholder and can be used to explicitly include TypeScript files in larger projects.
   - **`references`:** This section is typically used in **project references**, allowing you to split your project into multiple configurations. This setup points to `tsconfig.app.json` and `tsconfig.node.json`, which would allow separate configurations for your frontend (app) and server-side (Node.js) environments if needed. However, if you don't have these files yet, they are placeholders and optional unless you need more modular or environment-specific configurations.

2. **`baseUrl` and `paths`**:
   - **`baseUrl`:** Sets the base directory for module resolution. By setting this to `"."` (the root of your project), you can use simplified import paths.
   - **`paths`:** This introduces an alias system that allows you to import files using `@/` instead of lengthy relative paths. For example, instead of writing `../../components/button`, you can use `@/components/button`. This makes the code cleaner and easier to maintain.

---

### Do You Need `tsconfig.app.json` and `tsconfig.node.json`?

Since you don't have `tsconfig.app.json` and `tsconfig.node.json` files in your project, here's why:

1. **Optional for Most Projects**: These files are not mandatory unless your project requires multiple environments (e.g., separating frontend and backend configurations). Many projects can work just fine with a single `tsconfig.json` unless you have specific needs for isolating TypeScript configurations (e.g., one for the browser and one for Node.js).

2. **When You Might Need Them**:

   - **`tsconfig.app.json`:** You could create this if you want a separate TypeScript configuration for the frontend app. For example, if you have different settings for how TypeScript should behave in the frontend vs. other parts of the project.
   - **`tsconfig.node.json`:** This is useful if you're working on server-side code (Node.js) in the same project. It would let you use configurations like different module settings for backend Node.js code.

   If your project is purely frontend (React with Vite) and doesn’t involve server-side code in Node.js, you likely don’t need these files.

### **Conclusion**

- The `references` section is **optional** in your case. If you are not managing multiple environments, you can safely remove or ignore these.
- The important additions for you are **`baseUrl`** and **`paths`**, which provide path aliasing for simpler imports, making the project easier to scale and maintain.

If at some point you introduce server-side code or need more advanced configurations, you can consider adding `tsconfig.app.json` or `tsconfig.node.json` files.

# Notes

The 'tree' after the first `dfx deploy`, the node_modules and target dirs are not listed.

.
├── Cargo.lock
├── Cargo.toml
├── README.md
├── dfx.json
├── package-lock.json
├── package.json
├── src
│ ├── declarations
│ │ ├── ffs_backend
│ │ │ ├── ffs_backend.did
│ │ │ ├── ffs_backend.did.d.ts
│ │ │ ├── ffs_backend.did.js
│ │ │ ├── index.d.ts
│ │ │ └── index.js
│ │ └── ffs_frontend
│ │ ├── ffs_frontend.did
│ │ ├── ffs_frontend.did.d.ts
│ │ ├── ffs_frontend.did.js
│ │ ├── index.d.ts
│ │ └── index.js
│ ├── ffs_backend
│ │ ├── Cargo.toml
│ │ ├── ffs_backend.did
│ │ └── src
│ │ └── lib.rs
│ └── ffs_frontend
│ ├── dist
│ │ ├── assets
│ │ │ ├── index-2a8a0c6e.css
│ │ │ └── index-e536289f.js
│ │ ├── favicon.ico
│ │ ├── index.html
│ │ └── logo2.svg
│ ├── index.html
│ ├── package.json
│ ├── public
│ │ ├── favicon.ico
│ │ └── logo2.svg
│ ├── src
│ │ ├── App.jsx
│ │ ├── index.scss
│ │ ├── main.jsx
│ │ └── vite-env.d.ts
│ ├── tsconfig.json
│ └── vite.config.js
└── tsconfig.json

12 directories, 35 files

## DEFAULT DOCS FROM HERE

Welcome to your new `ffs` project and to the Internet Computer development community. By default, creating a new project adds this README and some template files to your project directory. You can edit these template files to customize your project and to include your own code to speed up the development cycle.

To get started, you might want to explore the project directory structure and the default configuration file. Working with this project in your development environment will not affect any production deployment or identity tokens.

To learn more before you start working with `ffs`, see the following documentation available online:

- [Quick Start](https://internetcomputer.org/docs/current/developer-docs/setup/deploy-locally)
- [SDK Developer Tools](https://internetcomputer.org/docs/current/developer-docs/setup/install)
- [Rust Canister Development Guide](https://internetcomputer.org/docs/current/developer-docs/backend/rust/)
- [ic-cdk](https://docs.rs/ic-cdk)
- [ic-cdk-macros](https://docs.rs/ic-cdk-macros)
- [Candid Introduction](https://internetcomputer.org/docs/current/developer-docs/backend/candid/)

If you want to start working on your project right away, you might want to try the following commands:

```bash
cd ffs/
dfx help
dfx canister --help
```

## Running the project locally

If you want to test your project locally, you can use the following commands:

```bash
# Starts the replica, running in the background
dfx start --background

# Deploys your canisters to the replica and generates your candid interface
dfx deploy
```

Once the job completes, your application will be available at `http://localhost:4943?canisterId={asset_canister_id}`.

If you have made changes to your backend canister, you can generate a new candid interface with

```bash
npm run generate
```

at any time. This is recommended before starting the frontend development server, and will be run automatically any time you run `dfx deploy`.

If you are making frontend changes, you can start a development server with

```bash
npm start
```

Which will start a server at `http://localhost:8080`, proxying API requests to the replica at port 4943.

### Note on frontend environment variables

If you are hosting frontend code somewhere without using DFX, you may need to make one of the following adjustments to ensure your project does not fetch the root key in production:

- set`DFX_NETWORK` to `ic` if you are using Webpack
- use your own preferred method to replace `process.env.DFX_NETWORK` in the autogenerated declarations
  - Setting `canisters -> {asset_canister_id} -> declarations -> env_override to a string` in `dfx.json` will replace `process.env.DFX_NETWORK` with the string in the autogenerated declarations
- Write your own `createActor` constructor
