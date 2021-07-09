# How:

This directory was created as follows:

```sh
npm init @vitejs/app singlepage-frontend-example --template react
cd singlepage-frontend-example
npm install
npm i vite-plugin-singlefile

cat > vite.config.singlefile.js <<EOF
import { defineConfig } from "vite"
import { viteSingleFile } from "vite-plugin-singlefile"

export default defineConfig({
	plugins: [viteSingleFile()],
	build: {
		target: "esnext",
		assetsInlineLimit: 100000000,
		chunkSizeWarningLimit: 100000000,
		cssCodeSplit: false,
		brotliSize: false,
		rollupOptions: {
			inlineDynamicImports: true,
			output: {
				manualChunks: () => "everything.js",
			},
		},
	},
})
EOF

# edit the package.json file and add the following script:
# "build-single": "vite --config vite.config.singlefile.js build"
```


Then you can do `npm run dev` to develop and automatically re-compile your frontend as you save,
or you can do: `npm run build-single` and it generates a `dist/index.html` file which should contain all of your .js and .css.
