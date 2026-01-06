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


