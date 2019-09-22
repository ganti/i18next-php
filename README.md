i18next-php
===================

PHP class for basic [i18next](https://www.i18next.com) functionality.

  
## Features

- Support for [variables](http://i18next.com/pages/doc_features.html#interpolation)
- Support for [context](http://i18next.com/pages/doc_features.html#context)
- Support for [basic sprintf](http://i18next.com/pages/doc_features.html#sprintf)
- Support for [basic plural forms](http://i18next.com/pages/doc_features.html#plurals)
- Support for [multiline in JSON](http://i18next.com/pages/doc_features.html)

### Missing Features
- Missing [interval plurals](http://i18next.com/pages/doc_features.html#plurals)
- Formatting [date, datetime, time, strings](https://www.i18next.com/translation-function/formatting)

- Formatting
## Usage

### basic usage
```php
// init i18next instance
i18next::init('en');

// get translation by key
echo  i18next::getTranslation('animal.cat');
```
**Output**: cat

### substitution of variables
```json
"common": {
			"name_age" : "{{name}} is {{age}} years old"
		}
´´´

```php
echo  i18next::getTranslation('common.name_age', array('name' => "Elisa", "age" => 32));
```
**Output**:
``` 
	Elisa is 32 years old
 ```

### plural forms
There are diffrent ways on how to store plural forms in json

```json
"animal":{
	"dog": "dog",
	"dog_plural": "dogs",

	"cat": "{{count}} cat",
	"cat_plural": "{{count}} cats",

	"elephant": "{{count}} elephant",
	"elephant_0": "no elephants",
	"elephant_2": "{{count}} elephants",

	"spiderWithCount" : "{{count}} spider",
	"spiderWithCount_plural" : "{{count}} spiders",
	"spiderWithCount_plural_0" : "no spiders"
}
```

 ```php
// get translation by key with plural forms
echo  i18next::getTranslation('animal.cat', array('count' => 2));
echo  i18next::getTranslation('animal.cat', 2);
```
**Output**: 
 ```
	2 cats
	2 cats
 ```


 ```php
echo  i18next::getTranslation('animal.elephant', array('count' => 0));
echo  i18next::getTranslation('animal.elephant', array('count' => 1));
echo  i18next::getTranslation('animal.elephant', array('count' => 2));
echo  i18next::getTranslation('animal.elephant', array('count' => 100));
```
**Output**:
 ```
	no elephants
	1 elephant
	2 elephants
	100 elephants
```

### Context
By providing a context you can differ translations. Eg. useful to provide gender specific translations.
 ```json
"people":{
	"friend" : "A friend",
	"friend_female" : "A girlfriend",
	"friend_female_plural" : "{{count}} girlfriends",
	"friend_male" : "A boyfriend",
	"friend_male_0" : "no boyfriend",
	"friend_male_plural" : "{{count}} boyfriends"
}
```
```php
echo  i18next::getTranslation('people.friend');
echo  i18next::getTranslation('people.friend', array('context' => 'female'));
echo  i18next::getTranslation('people.friend', array('count' => 2, 'context' => 'female'));
echo  i18next::getTranslation('people.friend', array('count' => 0, 'context' => 'male'));
echo  i18next::getTranslation('people.friend', array('count' => 1, 'context' => 'male'));
echo  i18next::getTranslation('people.friend', array('count' => 33, 'context' => 'male'));
```
**Output**:
 ```
	A friend
	A girlfirend
	2 girlfriends
	no boyfriends
	A boyfriend
	33 boyfriends
```




## Methods

### i18next::init( string $languageKey [, string $path ] );

Loads translation files from given path. Looks for `translation.json` by default.

```php
i18next::init('en', 'my/path/');
// loads my/path/translation.json
```

You can also use variables and split namespaces and languages to different files.

```php
i18next::init('en', 'languages/__lng__/__ns__.json');
// loads languages/en/common.json, languages/fi/common.json, etc...
```

Method throws an exception if no files are found or the json can not be parsed.

### mixed i18next::getTranslation( string $key [, array $variables ] );

Returns translated string by key.
```php
i18next::getTranslation('common.cat', array('count' => 2, 'lng' => 'fi'));
```

### boolean i18next::existTranslation( string $key );
Checks if translated string exists.

### void i18next::setLanguage( string $language [, string $fallback ] );

Changes language.

### array i18next::getMissingTranslations();
Gets an array of missing translations.
```php
array(1) {
	[0]=> array(2) {
	["language"]=> string(2) "en"
	["key"]=> string(14) "common.unknown"
	}
}
```

## Multilines in JSON-arrays
You can have html content written with multilines in JSON File
```json
{
  "en": {
    "common": {
        "thedoglovers":["The Dog Lovers by Spike Milligan",
			"So they bought you",
			"And kept you in a",
			"Very good home"]
       }
  }
}
```

  

# Development
## Run Tests
`composer test`
