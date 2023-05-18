# Next.js Demo Project & APM Setup

## Guide

***Note: This Demo project is specially denotes the integration between [Middleware](https://middleware.io/) & [Vercel](https://vercel.com/). This demo can help you visualize your traces of Vercel-deployed projects on the Middleware platform.***

### Step 1: Next.js Installation

Run the command below in your terminal to install Next.js sample project, i.e. `my-app`:
```
npx create-next-app@latest
```

### Step 2: Install Next.js APM package

Now go to `my-app` directory and run the command below in your terminal to install Middleware's Next.js APM package:
```
npm install @middleware.io/agent-apm-nextjs
```

### Step 2: Modify the `next.config.js` file

As this feature is experimental, you need to explicitly opt-in by adding the following code to your **next.config.js** file:
```
const nextConfig = {
     ---
     ---
     experimental: {
         instrumentationHook: true
     }
     ---
     ---
}
module.exports = nextConfig
```

### Step 3: Create an `Instrumentation` file

Create a custom `instrumentation.ts` file in your project root directory, and add the following code:
```
// @ts-ignore
import { track } from '@middleware.io/agent-apm-nextjs';

export function register() {
    track({
        projectName: "<PROJECT-NAME>",
        serviceName: "<SERVICE-NAME>",
        target: "vercel",
    });
}
```

### Step 4: Integrate Middleware on Vercel 
 Now you can deploy your project on Vercel. Afterward, you need to integrate the [Middleware](https://vercel.com/integrations/middleware) from the marketplace. You can find more details [here](https://docs.middleware.io/docs/vercel).

***Unlock the power of seamless integration between Middleware and Vercel, and elevate your Next.js projects with advanced APM capabilities.*** 🚀