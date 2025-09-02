
It’s common that front-end UI and back-end APIs are decoupled and that you will want to call your back-end API knowing it is securely authenticated.

## **Set up Kinde**

For additional security we recommend you [register your endpoint as an API](/developer-tools/your-apis/register-manage-apis/) in Kinde.

## **Set up front end**

### **Audience**

If you have registered your API in Kinde as above, you will need to make sure to pass the `audience` as a parameter in your authentication url. If you are using our [React](/developer-tools/sdks/frontend/react-sdk/#audience) or [JavaScript](/developer-tools/sdks/frontend/javascript-sdk/#audience) SDK this is handled for you.

This ensures the access token you receive when the user signs in, will contain the `audience` claim.

### **Calling your API**

When you make the call to your API you will want to ensure the access token is sent in the headers. An example in React for a bookstore app might be:

```jsx
const { getAccessToken } = useKindeAuth();
const [books, setBooks] = useState([]);

const fetchBooks = async () => {
  try {
    const accessToken = await getAccessToken();
    const res = await fetch(`https://api.myapp.com/books`, {
      headers: {
        Authorization: `Bearer ${accessToken}`
      }
    });
    const {data} = await res.json();
    setBooks(data.books);
  } catch (err) {
    console.log(err);
  }
};
```

## **Setup back end**

Now that the token is being passed from the front end you will need to verify it when it hits your API.

### **Libraries**

We recommend that you use a library to verify your token. If you are using ExpressJS you can use [our library](/developer-tools/sdks/backend/express-sdk/#verify-jwt) or the OpenID Foundation has [a list of libraries for working with JWT tokens](https://openid.net/developers/jwt/).

### **JSON Web Key**

It’s likely the library you decide to use will require the url for your public JSON Web Key (also known as a jwks file).

The file can be found here:

`https://<your_subdomain>.kinde.com/.well-known/jwks`

### **Audience**

If you opted to register your API with Kinde as per the `Setup Kinde` step then you will need to make sure you pass the `audience` you registered on Kinde to whichever library you are using.
