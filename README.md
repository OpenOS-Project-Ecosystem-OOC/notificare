# notificare

[![Built with Ona](https://ona.com/build-with-ona.svg)](https://app.ona.com/#https://github.com/Interested-Deving-1896/notificare) [![KDE Eco](https://img.shields.io/badge/KDE%20Eco-certified-brightgreen?logo=kde&logoColor=white&style=flat-square)](https://eco.kde.org/) [![Blue Angel](https://img.shields.io/badge/Blue%20Angel-DE--UZ%20215-0055a4?style=flat-square)](https://www.blauer-engel.de/en/certification/criteria)


<!-- AI:start:what-it-does -->
_Description pending._
<!-- AI:end:what-it-does -->

## Architecture

<!-- AI:start:architecture -->
_Architecture documentation pending._
<!-- AI:end:architecture -->

## Install

<!-- Add installation instructions here. This section is yours — the AI will not modify it. -->

```bash
git clone https://github.com/Interested-Deving-1896/notificare.git
cd notificare
```

## Usage

<!-- Add usage examples here. This section is yours — the AI will not modify it. -->

## Configuration


The gem exposes three module-level knobs, all `mattr_accessor` on `ActiveJob::Notificare`:

| Knob | Default | Purpose |
|---|---|---|
| `ActiveJob::Notificare.authenticate_with` | `nil` | Lambda evaluated via `instance_exec` in `ExecutionsController` to guard the admin UI. Nil in production denies access. |
| `ActiveJob::Notificare.current_recipient_proc` | `nil` | Lambda evaluated via `instance_exec` inside the engine's controllers to resolve the current recipient. Falls back to `current_notificare_recipient`, then `current_user`. |
| `ActiveJob::Notificare.parent_controller` | `"ApplicationController"` | The constant name (string) the engine's `ApplicationController` inherits from. Set this if your app routes everything through a custom base controller (e.g. `Api::BaseController`). |

Set them in `config/initializers/active_job_notificare.rb`.

### Resolving the current recipient

The notifications controller needs to know who the "current recipient" is for every request. The engine's `ApplicationController` inherits from `::ApplicationController` by default, so the simplest approach is to define `current_notificare_recipient` in your own `ApplicationController`:

```ruby
# app/controllers/application_controller.rb
def current_notificare_recipient
  current_user  # or however you expose the signed-in user
end
```

The engine controller inherits this method and calls it automatically. If neither `current_notificare_recipient` nor `current_user` is defined, the engine raises `NotImplementedError` with a clear message pointing you here.

**Alternative — proc in an initializer:**

If you prefer not to touch your `ApplicationController`, set a proc instead. It is evaluated via `instance_exec` inside the engine's controller so it has full access to session state:

```ruby
# config/initializers/active_job_notificare.rb
ActiveJob::Notificare.current_recipient_proc = -> { current_account }
```

**Advanced — custom parent controller:**

If your app uses a non-standard base controller (e.g. `Api::BaseController`), tell the engine to inherit from it instead of `ApplicationController`:

```ruby
# config/initializers/active_job_notificare.rb
ActiveJob::Notificare.parent_controller = "Api::BaseController"
```

The engine's controllers will then inherit from that class, picking up any auth helpers it defines.

---

## CI

<!-- AI:start:ci -->
_CI documentation pending._
<!-- AI:end:ci -->

## Mirror chain

<!-- AI:start:mirror-chain -->
This repo is maintained in [`Interested-Deving-1896/notificare`](https://github.com/Interested-Deving-1896/notificare) and mirrored through:

```
Interested-Deving-1896/notificare  ──►  OpenOS-Project-OSP/notificare  ──►  OpenOS-Project-Ecosystem-OOC/notificare
```

Changes flow downstream automatically via the hourly mirror chain in
[`fork-sync-all`](https://github.com/Interested-Deving-1896/fork-sync-all).
Direct commits to OSP or OOC are detected and opened as PRs back to `Interested-Deving-1896`.
<!-- AI:end:mirror-chain -->

## Contributors

<!-- AI:start:contributors -->
| Contributor | Commits |
|---|---|
| [@joaoGabriel55](https://github.com/joaoGabriel55) | 19 |
| [@Interested-Deving-1896](https://github.com/Interested-Deving-1896) | 1 |
<!-- AI:end:contributors -->

## Origins

<!-- AI:start:origins -->
_Original project — no upstream influences recorded._
<!-- AI:end:origins -->

## Resources

<!-- AI:start:resources -->
_No additional resource files found._
<!-- AI:end:resources -->

## Accessibility

<!-- AI:start:accessibility -->
This repo uses automated accessibility auditing via `check-accessibility.yml`.

Checks include: CODEOWNERS ownership coverage, README screen-reader compatibility,
WCAG 2.1 AA HTML compliance, audio overview (espeak-ng), and Braille output (liblouis).




Run the [Check Accessibility](https://github.com/Interested-Deving-1896/notificare/actions/workflows/check-accessibility.yml)
workflow to generate the first report and accessibility artifacts.
See the [W3C Web Content Accessibility Guidelines (WCAG)](https://www.w3.org/WAI/standards-guidelines/wcag/)
for the underlying accessibility reference.
<!-- AI:end:accessibility -->

## License

<!-- AI:start:license -->
[MIT](https://github.com/Interested-Deving-1896/notificare/blob/main/LICENSE) © 2026 [Interested-Deving-1896](https://github.com/Interested-Deving-1896)
<!-- AI:end:license -->
