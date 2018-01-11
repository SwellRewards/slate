## Populate Redemption Options
```javascript

  $(document).on("swell:setup", function(){

    // default option template
    swellAPI.populateSelectWithRedemptionOptions("select#redemption-options");

    // customize option template
    var tmpl = '<option class="redemption-option" value="{{id}}">{{name}} for {{costText}}</option>';
    swellAPI.populateSelectWithRedemptionOptions("select#redemption-options", tmpl);
  })
```
#### swellAPI.popuplateSelectWithRedemptionOptions(selectElSelector, optionTempate)

This method takes a select element selector and appends `<option>` elements for each redemption option.  You could do this yourself using swellAPI.getActiveRedemptionOptions() but this makes it a little simpler.

optionTemplate is an optional template string that uses {{key}} to dictate where to replace redemption option values.  The default string is `<option value="{{id}}">{{name}} ({{costText}})</option>`
