---
description: Specific information related to localizing the Rails app
---

# Server

The backend uses Transifex to manage translations, similar to the [Android workflow](android.md). The locale files sit in [`config/locales`](https://github.com/simpledotorg/simple-server/tree/master/config/locales).

On the transifex dashboard, click on `simple-server` &gt; `Resources` to look at the source files.

## Language mappings

In the GitHub integration settings \(`Settings` &gt; `Integrations`\), there is a language mapping section that tells Transifex how to convert from the locale conventions it uses to the locale conventions supported by Rails. This will be used by Transifex when raising pull requests to merge new translations into the app.

The current mapping \(at the time this was written\) looks something like this:

```yaml
settings:
  language_mapping:
    pa: pa_Guru_IN
    mr: mr_IN
    om: om_ET
    ti: ti_ET
```

On the left are locale codes in the Transifex convention, on the right are locales in the convention that Rails expects. Whenever a new language is added to the app, this mapping table also needs to be updated.

## Notes

Once a new locale file has been added to the project, it needs to be added to the list of [available locales](https://github.com/simpledotorg/simple-server/blob/9894516eec914397569af15b9964ec9bb1f20879/config/application.rb#L37) to be accepted by Rails.

