# Zerocheck examples

Zerocheck is an AI QA engineer that tests your app end to end. Verify critical flows and find bugs with a prompt.

These are generic examples: YAML tests to run with the CLI, and a checklist to paste into the web app. Adapt routes, labels and test data to your app.

What is here:

- `zerocheck.yaml`: project configuration with named environments.
- `release-checklist.md`: a checklist to paste into Import checklist in the web app; one browser test is drafted per item.
- `checks/`: five saved tests covering search, signup, a test-card checkout, a declined card and a password-reset request.
- `github/zerocheck.yml`: an example GitHub Actions job to adapt. The CLI does not generate it.

## Run them with the CLI

Requires Node.js 20.19 or newer and a dedicated Zerocheck instance, provisioned during hands-on onboarding: https://tryzerocheck.com/onboarding/. Copy `zerocheck.yaml` and the tests into your app's repository and set `project` to your project ID, then:

```bash
npm install --save-dev --save-exact zerocheck
npx playwright install chromium
export ZEROCHECK_API="https://your-instance.example.com"
npx zerocheck login
mkdir -p zerocheck
cp -R checks zerocheck/tests
```

Start with a test that needs no credentials:

```bash
npx zerocheck run zerocheck/tests/search-finds-a-product.yaml --env dev
```

Before running all five, set up the credentials two of them use. `signup-creates-a-workspace.yaml` and `password-reset-request.yaml` use the secret names `TEST_PASSWORD` and `TEST_EMAIL`, so add them under your environment's `secrets` in `zerocheck.yaml` and set the variables they name. Then:

```bash
npx zerocheck run --env dev
```

To use the checklist, open your instance's web app and choose Import checklist.

## Rules the examples follow

- Every test ends with a `Verify` step that names a visible outcome.
- Test data uses placeholders such as `{{unique_email}}`; credentials use `${NAME}` references configured in `zerocheck.yaml` or the web app, never literal secrets.
- Payment tests use the provider's test mode and public test cards. Embedded checkout frames need no origin list: `allowed_origins` is an optional restriction, and an environment that sets one must include the payment frame's origin.
- Nothing here verifies an inbox, a webhook or a settlement. Keep those in your own integration tests.

Reference: https://tryzerocheck.com/docs/test-format/ and https://tryzerocheck.com/llms.txt
