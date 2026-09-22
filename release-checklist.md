# Release smoke tests
Note: A product named Notebook exists in the test environment.
Note: Payments run in the provider's test mode with public test cards.

- Search finds a product
  Open the app.
  Search for "Notebook".
  Verify text "Notebook" is visible.

- A new user can create a workspace
  Open /signup.
  Sign up with a fresh email address and the name "Zerocheck Test".
  Verify text "Welcome" is visible.

- A customer can upgrade with a test card
  Open /pricing and choose Upgrade to Pro.
  Pay with the card 4242 4242 4242 4242, any future expiry and any CVC.
  Verify text "Payment successful" is visible.

- A declined card shows an error and grants nothing
  Open /pricing and choose Upgrade to Pro.
  Pay with the card 4000 0000 0000 0002.
  Verify text "Your card was declined." is visible and "Payment successful" is not visible.

- The help page is available
  Open /help.
  Verify text "Contact support" is visible.
