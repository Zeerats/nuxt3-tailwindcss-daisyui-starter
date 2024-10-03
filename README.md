# Nuxt 3 Template with TailwindCSS, DaisyUI, and Laravel Sanctum Auth

This is a starter template for building a Nuxt 3 project with TailwindCSS and DaisyUI for styling, Roboto Mono as the default font, and Laravel Sanctum authentication already set up to work with a Laravel API as the backend.

## Features

- **Nuxt 3** for a modern Vue.js development experience.
- **TailwindCSS** for utility-first CSS styling.
- **DaisyUI** for pre-built UI components on top of TailwindCSS.
- **Roboto Mono** set as the default font for a sleek, mono-spaced look.
- **Laravel Sanctum Authentication** fully integrated with a Laravel API backend.

## Installation

1. **Clone the repository**:
    ```bash
    git clone <your-repository-url>
    cd <your-repository-directory>
    ```

2. **Install dependencies**:
    ```bash
    npm install
    ```

3. **Configure environment variables** (depracated):
    - Create a `.env` file in the project root with the following environment variables:
      ```bash
      NUXT_API_BASE_URL=http://localhost:80/api
      NUXT_PUBLIC_BACKEND_URL=http://localhost:80
      ```

4. **Run the development server**:
    ```bash
    npm run dev
    ```

## TailwindCSS & DaisyUI

This project includes TailwindCSS and DaisyUI for a customizable and responsive design. 

TailwindCSS is configured with a custom font (`Roboto Mono`) set as the default:

```css
/* tailwind.config.js */
module.exports = {
  theme: {
    extend: {
      fontFamily: {
        mono: ['Roboto Mono', 'monospace'],
      },
    },
  },
  plugins: [require('daisyui')],
};
```

## Laravel API Backend

The backend is a Laravel 11 API that serves as the authentication provider using Sanctum. Here's a quick rundown of the API routes and configuration.

### API Routes

```php
// Login route
Route::post('/login', [AuthController::class, 'login']);

// Logout route (requires authentication)
Route::post('/logout', [AuthController::class, 'logout'])
    ->middleware('auth:sanctum');

// Authenticated user route (requires authentication)
Route::get('/user', function (Request $request) {
    return $request->user();
})->middleware('auth:sanctum');
```

### CORS Configuration

The following CORS settings allow your Nuxt app (running on `http://localhost:3000`) to communicate with your Laravel API:

```php
'paths' => ['api/*', 'sanctum/csrf-cookie'],
'allowed_methods' => ['*'],
'allowed_origins' => ['http://localhost:3000'],
'allowed_origins_patterns' => [],
'allowed_headers' => ['*'],
'exposed_headers' => [],
'max_age' => 0,
'supports_credentials' => true,
```

### Stateful Middleware Configuration

In Laravel 11, the stateful middleware configuration is now defined within `bootstrap/app.php`. The setup is as follows:

```php
    ->withMiddleware(function (Middleware $middleware) {
        $middleware->statefulApi();
    })
```

### Sanctum Setup

Sanctum now comes pre-installed with Laravel 11, so no additional setup is required. There’s no need to register a provider or install any additional packages.

## Authentication Flow

1. **Login**: The Nuxt frontend uses Sanctum's `/login` route to authenticate users.
2. **Session Handling**: Sanctum manages the session via cookies, enabling secure, stateful authentication.
3. **Logout**: The `/logout` route invalidates the user's session.
4. **Fetch User**: Once authenticated, the frontend can retrieve the logged-in user via the `/user` route.

## License

This project is open-source and licensed under the [MIT License](LICENSE).
