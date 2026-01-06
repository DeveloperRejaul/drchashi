# i18 implement in react native with online sync

### step: 1
inatll dependency
```bash 
yarn add i18next react-i18next i18next-http-backend
```
### step:2 i18 config 

```ts
i18n.use(HttpBackend)
  .use(initReactI18next)
  .init({
    fallbackLng: 'bn',
    debug: true,

    backend: {
      loadPath: 'https://raw.githubusercontent.com/DeveloperRejaul/drchashi/main/locales/{{lng}}/{{ns}}.json',
    },

    interpolation: {
      escapeValue: false,
    },

    react: {
      useSuspense: true,
    },

    ns: ['common'],
    defaultNS: 'common',
  });

export default i18n;
```
### step:2 i18 config get data form api
```js
/* eslint-disable import/no-named-as-default-member */
import i18n from 'i18next';
import { initReactI18next } from 'react-i18next';
import HttpBackend, { HttpBackendOptions } from 'i18next-http-backend';

let len = 'bn';
i18n.use(HttpBackend)
  .use(initReactI18next)
  .init<HttpBackendOptions>({
    fallbackLng: 'bn',
    backend: {
      loadPath: 'https://backend.drchashi.com/api/v1/configuration/configuration-data',
      request: async (options, url, payload, callback) => {
        try {
          const response = await fetch(url, {
            method: 'GET',
            headers: {
              'Content-Type': 'application/json',
              language:len,
            },
          });

          const data = await response.json();

          callback(null, {
            status: response.status,
            data,
          });
        } catch (error) {
          callback(error, null);
        }
      },
    },

    interpolation: {
      escapeValue: false,
    },

    react: {
      useSuspense: false,
    },

    ns: ['common'],
    defaultNS: 'common',
  });


export const changeLanguage = async (lng: 'en' | 'bn') => {
  len= lng;
  await i18n.reloadResources(lng)
  await i18n.changeLanguage(lng);
};
export default i18n;

```

### step 03 : Setup provide
```jsx 
export default function App() {
  return (
    <I18nextProvider i18n={i18n}>
      <HomeScreen />
    </I18nextProvider>
  );
}
```

### step 04: use i18

```tsx
const HomeScreen = () => {
  const { t , i18n} = useTranslation();

  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Text>{t('welcome')}</Text>
      <Text>{t('login')}</Text>
      <TouchableOpacity 
        onPress={()=>{
          if(i18n.language === 'en') {
            i18n.changeLanguage('bn')
            return
          }
          i18n.changeLanguage('en')
        }}>
        <Text>Change Lan</Text>
      </TouchableOpacity>
    </View>
  );
};
```


