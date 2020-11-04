## ooz-wasm

WASM bindings for [ooz](https://github.com/powzix/ooz): Open source Kraken, Mermaid, Selkie, Leviathan, LZNA, Bitknit decompressor.

### Note

`.wasm` file is base64 embedded using `-s SINGLE_FILE=1`.

Requires browser WebAssembly SIMD support.

- Can be enabled in Chromium-based browsers on `about://flags/#enable-webassembly-simd`
- Can be enabled in Firefox on `about:config` `javascript.options.wasm_simd`

### Usage

```ts
// Compile and instantiate WebAssembly code
// NOTE: called implicitly by all other methods
function load(): Promise<void>;

// Decompress data
// NOTE: returned TypedArray lives in WASM memory, you can safely use it until the next call to decompressUnsafe/decompress
function decompressUnsafe(
  data: Uint8Array,
  rawSize: number
): Promise<Uint8Array>;

// Decompress data
function decompress(data: Uint8Array, rawSize: number): Promise<Uint8Array>;
```

### Build using native tools

1. Install Emscripten SDK
2. `emcmake cmake -B build [-G "..."]`
3. Build
   ```bash
   # Ninja (-G "Ninja")
   cmake --build build
   # or Makefiles (-G "Unix Makefiles")
   emmake make -C build
   ```

### Build Using Docker

1. Install Docker
2. Build the image
   ```PowerShell
   docker build -t ooz .
   ```
3. Run the build commands in the container
   ```PowerShell
   docker run --rm -it --mount type=bind,source="${PWD}",target="/src" emscripten/emsdk bash -c "cd /src; emcmake cmake -B build; cmake --build build"
   ```
