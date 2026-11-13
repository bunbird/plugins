## When to use

Use this when the person wants a small web app, tool, tracker or form built and hosted for them or a group: a holiday request board, a shared checklist, a sign-up sheet. It is for apps people keep using, not a one-off snippet to paste into a chat.

## One connector

There is one MCP connector, **{{name}}** ({{url}}). Access is by invitation. Connecting signs the person in, and every tool then works for them. Apps belong to an organisation, and each draft is made in one of the person's organisations and published there. Tool names below are the current ones; if your tool list shows different names, go by each tool's description and the server's own instructions.

If the connector is not connected, tell the person to connect it and approve the sign-in. Do not try to work around it.

## The flow

1. **Read the guide first.** Call `read_guide` with no topic (or read the resource `bunbird://guide`) for the overview, then once per capability the app uses (`read_guide({ topic })`). Take the topics from the overview, not from memory. The guide matches the SDK the code is compiled with and is the only reference. Do not rely on what you remember of Bunbird.
2. **Pick the organisation.** `list_organisations` says which ones the person is in and what they may do in each. When there are several, ask which.
3. **Create one draft per app** (`create_draft`) in that organisation, with a name and a one-line description in the person's words. Every later call names the draft by its `draftAppId`.
4. **Build** (`build_draft`) with every file of the app the first time, then only the files that changed, over `base`. Use `stage_files` first when the files do not fit in one call. A failed build is an answer, not a refusal: `problems` lists every problem at once, so fix all of them before building again. A successful build also reports what publishing would object to, so read that too.
5. **Preview.** Give the person the preview URL and let them try it. Change what they ask for and build again.
6. **Publish** (`publish_draft`, with the `deployId` of the build to ship) only when the person asks. Publishing ends the draft and its preview URL, so confirm first. The guide's `publish` topic has the details.

## Carry on, change, share

- `list_drafts` lists the drafts to carry on with in another conversation, and `read_sources` returns a draft's files by its `draftAppId`.
- A draft falls asleep at its `sleepsAt`: its preview stops and its test data goes, its files stay. `wake_draft` wakes it for a short while, to finish and publish it.
- To let somebody outside the organisation try a draft before it is published, `create_draft_link` gives a link to its preview, only when the person asks. It stops working when the draft falls asleep.
- To change a published app, `iterate_app` starts a draft from it and leaves the app untouched. The guide's `publish` topic says who may and what happens to the app's data.
- `list_apps` lists the published apps by the `appId` the other tools take. `read_app_access` says who opens one and with which roles.
- `invite_member` brings somebody into one of the person's organisations, as an admin or a member. `invite_guest` lets somebody from outside into one published app. `set_app_roles` changes who opens an app with which roles. These, and creating or deleting organisations, apps and drafts, happen only when the person asks for them. Read the guide's `manage` topic first.
- `list_notifications` says what needs the person's attention: something about to be deleted, a secret that stopped working, an invitation, a change somebody else made.
- A tool that refuses says why: the platform's status first, then one objection per line. Fix what it names, or tell the person.

## Do not

- Do not invent APIs, spec fields or SDK calls. If the guide does not say it exists, it does not.
- Do not paste a credential into chat, code, or an app's files.
- Do not create a new draft to fix a failed build. Fix the files and build again on the same draft.
- Do not publish, delete or invite anyone unless the person asked for it.
- Do not choose an organisation for the person when they are in several.
- When neither the tools nor the guide do what the person asks, call `report_missing_capability` once, then tell them it cannot be done from here yet. Do not fake it.

## More

Agent docs: {{llms}} and {{llmsFull}}. The skill index is at {{agentSkills}}. A prompt that walks a person through a first app is at https://app.bunbird.com/docs/prompts/build-your-first-app.md.
