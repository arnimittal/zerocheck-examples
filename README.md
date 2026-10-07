# Zerocheck examples

Generic, runnable examples of Zerocheck tests: plain-English YAML that a QA agent runs in a real browser and reruns locally, in GitHub Actions or in a hosted browser. Adapt routes, labels and test data to your app.

What is here:

- `zerocheck.yaml`: project configuration with named environments.
- `release-checklist.md`: a checklist to import; one browser test is drafted per item.
- `checks/`: five saved tests covering search, signup, a test-card checkout, a declined card and a password-reset request.
- `.github/workflows/zerocheck.yml`: the GitHub Actions workflow the CLI generates, which runs the tests on every pull request and updates one comment.

## Run them

Requires Node.js 20.19 or newer and a Zerocheck project. Access is provisioned on an onboarding call; start at https://tryzerocheck.com/onboarding/.

```bash
npm install --save-dev --save-exact zerocheck@0.1.9
npx zerocheck login
npx zerocheck init --url http://localhost:3000
npx zerocheck install
cp -R checks zerocheck/tests
npx zerocheck validate
npx zerocheck run --env dev --runner local
```

Or import the checklist and review one drafted test per item before saving:

```bash
npx zerocheck import release-checklist.md --env dev --runner local
npx zerocheck import --draft DRAFT_ID
```

## Rules the examples follow

- Every test ends with a `Verify` step that names a visible outcome.
- Test data uses placeholders such as `{{unique_email}}`; credentials use `${NAME}` references configured in `zerocheck.yaml` or the web app, never literal secrets.
- Payment tests use the provider's test mode and public test cards. Embedded checkout frames need no origin list: `allowed_origins` is an optional restriction, and an environment that sets one must include the payment frame's origin.
- Nothing here verifies an inbox, a webhook or a settlement. Keep those in your own integration tests.

Reference: https://tryzerocheck.com/docs/test-format/ and https://tryzerocheck.com/llms.txt
