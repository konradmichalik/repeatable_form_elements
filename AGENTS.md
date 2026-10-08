# AGENTS.md

## Project overview

TYPO3 extension `repeatable_form_elements` (Composer `tritum/repeatable-form-elements`, type `typo3-cms-extension`). It adds a "Repeatable container" element to the TYPO3 form framework. Editors put any fields into the container, and frontend users can duplicate and remove it. Validators and property mapping are copied with each duplicate, and finishers are aware of the copies.

- TYPO3 `^12.4 || ^13.4` (`typo3/cms-core`, `cms-extbase`, `cms-form`), no explicit PHP constraint in `composer.json`
- Namespace `TRITUM\RepeatableFormElements\` maps to `Classes/`
- GPL-2.0-or-later

## Structure

- `Classes/FormElements/`: `RepeatableContainer` (extends the form `Section` class) and `RepeatableContainerInterface`
- `Classes/Service/CopyService.php`: duplication of form elements and variants
- `Classes/Finisher/SaveToDatabaseFinisher.php`: extended database finisher that handles repeated elements
- `Classes/Event/CopyVariantEvent.php` and `Classes/EventListener/`: PSR-14 event to extend or disable variant copying, and the listener adapting variant conditions
- `Classes/Hooks/FormHooks.php`, `Classes/Configuration/Extension.php`
- `Configuration/`: `Services.yaml`, `Yaml/FormSetup.yaml` and `FormSetupBackend.yaml`, `Sets/RepeatableFormElements/` (TYPO3 v13 site set), `TCA/Overrides/sys_template.php` (static template), `TypoScript/setup.typoscript`, `JavaScriptModules.php`, `Icons.php`
- `Resources/Private/Frontend/Partials/RepeatableContainer.html`: frontend template
- `Resources/Public/JavaScript/frontend/repeatable-container.js`: add and remove buttons in the frontend
- `Resources/Public/JavaScript/backend/form-editor/view-model.js`: form editor integration
- `Resources/Private/ExampleFormDefinitions/`: example form definition for the extended finisher
- `ext_emconf.php`, `ext_localconf.php`, `README.md`

## Development commands

There is no build step. PHP is autoloaded through Composer, JavaScript is vanilla and unbundled.

```bash
composer install
```

## Testing

No test suite, test configuration or CI workflow exists on the default branch. Verify changes manually in a TYPO3 instance by building a form in the form editor and submitting it in the frontend, including validation and the finisher.

## Code style and linting

No linter, formatter or static analysis is configured. Follow the style of the surrounding files.

- Container properties: `minimumCopies` (default 0), `maximumCopies` (default 10), `showRemoveButton` (default true), `elementClassAttribute` (default `repeatable-container`)
- Extend copy behavior through `CopyVariantEvent`, copying of variants can be switched off with the feature `repeatableFormElements.copyVariants`
- Wire services through `Configuration/Services.yaml` dependency injection
- Keep the frontend JavaScript dependency free (jQuery was removed)
- Keep the version constraints of `composer.json` and `ext_emconf.php` in sync

## Git workflow

- Commit format: `<type>: <description>` with type one of `feat`, `fix`, `refactor`, `docs`, `test`, `chore`, `perf`, `ci`
- Describe the change, not what prompted it
- No co-author trailers
