# No IDP Login Required
```mermaid
sequenceDiagram
    participant User as End User / Web Browser
    participant PWA as PWA (Client)
    participant IDP as Identity Provider
    participant API as Backend API

    Note over User,IDP: User is already logged in with the Identity Provider (e.g., Google's SSO)
    User->>PWA: User visits the PWA's URL
    PWA->>User: PWA checks for existing session. Finds none.<br>1. Redirect to IDP Authorization Endpoint

    Note over User,IDP: IDP's server recognizes the user's session cookie.<br>No login required.
    IDP-->>User: 2. Redirect back to PWA with Authorization Code

    User->>PWA: 3. PWA loads and extracts Authorization Code
    PWA->>IDP: 4. POST to Token Endpoint <br>(Authorization Code + Code Verifier)

    IDP-->>PWA: 5. JWT Access Token + ID Token
    Note left of PWA: PWA stores the tokens for the current session.

    PWA->>API: 6. API Call (e.g., GET /api/user/profile)<br>Header: Authorization: Bearer <JWT Access Token>
    Note right of API: Backend API verifies JWT and responds.

    API-->>PWA: 7. Response with Protected User Data
    PWA->>User: 8. Display Main Application Page
    Note over PWA, User: The PWA renders the user's content,<br>providing a "seamless" authenticated experience.

    ```
