# **React.js Project Rules**

## **Required Packages**

```
{
  "dependencies": {
    "@shadcn/ui": "^0.0.4",
    "@testing-library/jest-dom": "^5.17.0",
    "@testing-library/react": "^13.4.0",
    "@testing-library/user-event": "^13.5.0",
    "autoprefixer": "^10.4.20",
    "axios": "^1.7.7",
    "crypto-js": "^4.2.0",
    "lucide-react": "^0.453.0",
    "moment": "^2.30.1",
    "postcss": "^8.4.47",
    "react": "^18.3.1",
    "react-chat-elements": "^12.0.16",
    "react-dom": "^18.2.0",
    "react-router-dom": "^6.27.0",
    "react-scripts": "5.0.1",
    "socket.io-client": "^4.8.0",
    "tailwindcss": "^3.4.14",
    "web-vitals": "^2.1.4",
    "zustand": "^5.0.1"
  },
  ...
}
```

* node version: v18.17.0
* 명시된 모든 Package의 버전을 그대로 사용해서 프로젝트를 생성해줘.

## **Other Configuration Notes**

* Remove the `App.css` file and the related `import` statement in `App.js`.
* Delete unnecessary files such as `logo.svg`.

## **Example Folder Structure**

```
react-app
├── public
├── src
│   ├── api
│   │   ├── index.js
│   │   └── featureApi.js
│   ├── components
│   │   └── LoadingSpinner.js
│   ├── config
│   ├── pages
│   │   ├── Home.js
│   │   └── Feature.js
│   ├── services
│   ├── stores
│   │   ├── loadingStore.js
│   │   └── featureStore.js
│   ├── styles
│   └── utils
├── assets
│   ├── fonts
│   └── images
├── constants
├── hooks
├── layouts
├── update-build-version.js
├── tailwind.config.js
├── postcss.config.js
└── package.json
```

* **src/**
  * **api**: API communication logic
  * **components**: Reusable UI components
  * **config**: Environment configuration files
  * **pages**: Route-level page components
  * **services**: Global business logic and third-party integration
  * **stores**: State management (e.g., Zustand, Redux store)
  * **styles**: Global styles and theme configuration (including custom CSS/SCSS)
  * **utils**: Utility and helper functions
* **assets/**
  * **fonts/**: Custom fonts
  * **images/**: Static images used in the project
* **constants/**: Project-wide constants (e.g., API endpoints, event keys, theme values)
* **hooks/**: Custom React hooks (e.g., `useInput`, `useDebounce`, `useFetch`)
* **layouts/**: Common page layouts (e.g., Header, Footer, Sidebar)
* **update-build-version.js**: Script to append a timestamp version to assets during build
* **tailwind.config.js**: Tailwind CSS configuration
* **postcss.config.js**: PostCSS plugin configuration
* **package.json**: Project metadata and dependencies

## **API Code Guidelines**

* The preferred pattern is: `store` → `api call`
  * Response data should be stored in the `store` to share across components.
  * Store methods should return API results to support direct use and chaining.
* If the response data is not shared across pages and does not need to be held in state:
  * You may call the API directly within a component.

## **Axios Wrapper**

* Use an axios wrapper to eliminate duplicated code.
* Implement shared behavior such as loading state management.

```javascript
// src/api/index.js
import axios from 'axios';
import { useLoadingStore } from '../stores/loadingStore';

const api = axios.create({
  baseURL: '/api',
});

api.interceptors.request.use((config) => {
  useLoadingStore.getState().setIsLoading(true);
  return config;
});

api.interceptors.response.use(
  (response) => {
    useLoadingStore.getState().setIsLoading(false);
    return response;
  },
  (error) => {
    useLoadingStore.getState().setIsLoading(false);
    return Promise.reject(error);
  }
);

export default api;
```

## **`api/featureApi.js` Example**

* Under the `api` directory, create separate API files for different feature modules.
* These files should import the shared `axios` instance (`api`) to make requests.

```javascript
import api from './index';

export const getFeatureData = async (featureId) => {
  try {
    const response = await api.get(`/features/${featureId}`);
    return response.data;
  } catch (error) {
    console.error('Failed to fetch feature data:', error);
    throw error;
  }
};
...
```

## **`stores/featureStore.js` Example**

  * This file demonstrates how to create a store using Zustand.
  * The store encapsulates state and the actions that manipulate that state.
  * Actions call functions from `featureApi.js` to communicate with the backend and update the state based on the API results.

```javascript
// src/stores/featureStore.js
import { create } from 'zustand';
import {
  getFeatureData,
  ...
} from '../api/featureApi';

export const useFeatureStore = create((set) => ({
  featureData: null,
  error: null,

  fetchFeature: async (featureId) => {
    set({ error: null });
    try {
      const data = await getFeatureData(featureId);
      return data;
    } catch (error) {
      set({ error: error.message });
      console.error('Error fetching feature:', error);
      throw error;
    }
  },

  ...

  clearError: () => {
    set({ error: null });
  },
}));
```

## **Naming Conventions**

1. **Components**: Use `PascalCase` (e.g., `SubmitButton`, `UserProfile`).
2. **Files**:
  * Component files: `PascalCase` (e.g., `SubmitButton.js`).
  * Others (hooks, utils, etc.): `camelCase` (e.g., `useUserData.js`, `formatDate.js`).
3. **Functions/Variables**: Use `camelCase` (e.g., `userData`, `fetchData`).
4. **Custom Hooks**: Use `use` prefix (e.g., `useAuth`, `useForm`).
5. **Event Handlers**: Use `handle` prefix (e.g., `handleClick`, `handleSubmit`).