# BitMonk

## BitMonk - A Self-Custody Open Source Bitcoin Wallet within Telegram

BitMonk is a self-custody Bitcoin wallet integrated within Telegram, leveraging modern web technologies to provide a seamless user experience.

## Tech Stack
- **SvelteKit**
- **Static Badge**
- **TailwindCSS**
- **Material You**
- **Zig Lang**
- **WebAssembly**
- **BlockStream**
- **Grammy**
- **PocketBase**

## Run Locally

### Requirements
- **Yarn** installed
- **Telegram bot** registered via [@BotFather](https://telegram.me/BotFather)

### Instructions

1. **Clone this repository:**

   ```sh
   git clone https://github.com/DavisDmitry/BitMonk
   ```

2. **Install dependencies:**

   ```sh
   yarn
   ```

3. **Copy .env.example to .env:**

   ```sh
   cp .env.example .env
   ```

4. **Make the app publicly available:**

   ```sh
   yarn expose
   ```

5. **Edit your .env file** (see Configuration section below).

6. **Build the app:**

   ```sh
   yarn build
   ```

7. **Set the webhook URL** (replace `<VALUE>` with values from Configuration):

   ```sh
   curl -d "url=<PUBLIC_APP_URL>/api/bot&secret_token=<WEBHOOK_SECRET>" -X POST https://api.telegram.org/bot<BOT_TOKEN>/setWebhook
   ```

8. **Run the app:**

   ```sh
   yarn preview
   ```

## Configuration

Everything is configured using environment variables. Settings are divided into two types:

- **Static (build time)**
- **Dynamic (runtime)**

### Variables

- **PUBLIC_NET**: Bitcoin network (`main` or `test`) - static
- **PUBLIC_BOT_USERNAME**: Username of your bot (without `@`) - static
- **PUBLIC_APP_URL**: Base path of your mini app - static (must start with `https://`)
- **BOT_TOKEN**: Token of your bot - dynamic (get it from [@BotFather](https://telegram.me/BotFather))
- **WEBHOOK_SECRET**: Secret token for webhooks from Telegram Bot API - dynamic
- **PB_URL**: PocketBase backend URL - dynamic, optional (set empty string if not needed)
- **PB_EMAIL**: PocketBase admin email - dynamic, required if `PB_URL` provided (set empty string if not needed)
- **PB_PASSWORD**: PocketBase admin password - dynamic, required if `PB_URL` provided (set empty string if not needed)

### Optional Backend
If you want to save bot users to a database, you can run the PocketBase backend. The bot will work without it.

#### Instructions

1. **Download PocketBase binary**

2. **Apply migrations (stored in `pb_migrations`):**

   ```sh
   ./pocketbase migrate
   ```

3. **Start the web server:**

   ```sh
   ./pocketbase server
   ```

4. **Go to PocketBase admin UI and set up an admin account**

5. **Configure `PB_*` variables in your .env**

## Production Deploy
BitMonk can be deployed like any other SvelteKit application. See [SvelteKit documentation](https://kit.svelte.dev/docs) for details.

## For Developers
WASM code for signing transactions has already been compiled (stored in `static`). You can also compile it yourself if you have Zig installed.

```sh
yarn build-zig
```

## Supported Telegram Mini App Features
- **Scanning a QR-code**: `src/routes/wallet/send/+page.svelte`
- **Popups, closing confirmation**
- **Dynamic theme changing**: `src/routes/+layout.ts`
- **Main button**
- **Back button**
- **Haptic feedback**

## Current State and Limitations
- **BIP-44 and BIP-84** have been partially implemented. BitMonk creates a hierarchy of keys but uses only one account and one key for incoming transactions and change (`m/84'/0'/0'/0/0` for mainnet, `m/84'/1'/0'/0/0` for testnet).
- **Only one address type** can be created: Native SegWit (bech32).
- **Only one type of outgoing transaction** is supported: P2WPKH (native SegWit).
- The **seed phrase cannot be imported** and is generated randomly when the Mini App is first launched.
- The seed phrase is stored in `localStorage` (similar to TON Space, but without cloud backup capability).

Due to these limitations, the mainnet version has not yet been launched.