
A common pattern in B2B products is for users who belong to multiple organizations to be able to switch between them. This topic demonstrates how to achieve this.

## Step 1: Add org data to ID tokens

The first step is to include a list of organizations a user belongs to, in their ID token.

1. In Kinde, open the application you want to enable a switcher for. For example, go to **Settings > Applications > [View details] > Tokens.**
2. Scroll down to the **Token customization** section and select **Configure** on the **ID token** card. The **Customize ID token** window opens.

<img
  src="https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/60ed313e-8250-4b33-0645-51a97ccc6d00/public"
  alt=""
  width="672px"
  height="auto"
  fetchpriority="low"
  loading="lazy"
  decoding="async"
/>

3. Select the **Organizations (array)** checkbox in the **Additional claims** section.
4. Select **Save**. This adds the organization `id` and `name` to the user’s ID token, in the following format:

```jsx
"organizations": [
    {
      "id": "org_4ba6821b521",
      "name": "Golden Finance"
    },
    {
      "id": "org_b7226a3b5f0",
      "name": "UTM Bank"
    },
    {
      "id": "org_16374a4fc3f",
      "name": "Trueblue Pty Ltd"
    }
  ]
```

You can now extract the `organizations` claim from ID tokens in the way you normally would. Typically the SDK you are using will have a method for this.

For example, in React you could use:

```jsx
const { getClaim } = useKindeAuth()

getClaim('organizations', 'idToken').then((organizations) => {
  console.log('value:', organizations?.value)
})
```

## Step 2: Build the switcher

To build a simple list of orgs, use something like the following React example. You’ll need to include a call to the `login` method for each organization, passing in the id.

In this example, we’ve also included a check to see if this is the current organization.

```jsx
const { login, getClaim, getOrganization } = useKindeAuth()

const [orgs, setOrgs] = useState<{ id: string; name: string }[]>([])
const [currentOrgCode, setCurrentOrgCode] = useState<string | null>(null)

useEffect(() => {
  getClaim('organizations', 'idToken').then((organizations) => {
    setOrgs(organizations?.value ?? [])
  })
  getOrganization().then((org) => {
    setCurrentOrgCode(org)
  })
}, [getClaim, getOrganization])

<ul>
  {orgs.map((item) => (
    <li key={item.id}>
      <button onClick={() => login({ orgCode: item.id })} type='button'>
        {item.name}
        {currentOrgCode === item.id ? ' (Current)' : null}
      </button>
    </li>
  ))}
</ul>
```

With some extra styling, a switcher might look something like this:

<img
  src="https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/762446ab-6ce7-4e6f-746f-d2ca87efee00/public"
  alt=""
  width="672px"
  height="auto"
  fetchpriority="low"
  loading="lazy"
  decoding="async"
/>
