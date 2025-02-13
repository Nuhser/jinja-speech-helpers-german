# Jinja Speech Helper Macros for German Text

[![Version](https://img.shields.io/github/v/release/Nuhser/jinja-speech-helpers-german)](https://github.com/Nuhser/jinja-speech-helpers-german/releases/latest)
[![Last commit](https://img.shields.io/github/last-commit/Nuhser/jinja-speech-helpers-german)](https://github.com/Nuhser/jinja-speech-helpers-german/commits/master/)
[![GitHub Repo stars](https://img.shields.io/github/stars/Nuhser/jinja-speech-helpers-german)](#)
<br/>
[![HACS](https://img.shields.io/badge/HACS-Default-41BDF5.svg?logo=home-assistant-community-store)](https://github.com/hacs/default)
<br/>
[![Home Assistant Community Forum](https://img.shields.io/badge/Home_Assistant-Community_Forum-18BCF2?logo=home-assistant)](https://community.home-assistant.io/t/custom-template-for-formatting-german-text/845188)

> [!IMPORTANT]
> This template is based on a template idea by [@jazzyisj](https://github.com/jazzyisj). Please check out their repository for a version that works with English text and leave a star: https://github.com/jazzyisj/speech-helpers-jinja/
>
>  <br>
>
> You can also find it in inside HACS in Home Assistant under the name *Jinja Speech Helpers*.

This template contains Jinja macros that work with German text. One use-case for those macros are [notification messages](https://companion.home-assistant.io/docs/notifications/notifications-basic/) where you don't know if a word needs to be singular or plural. The macros will look at the given quantity and use the correct words.

There are more macros for other formatting needs. Check out the documentation below.

## Installation

<details>

<summary>Without HACS</summary>

<br>

1. Download the contents from [speech_helpers_german.jinja](https://github.com/Nuhser/jinja-speech-helpers-german/blob/master/speech_helpers_german.jinja) from the master-branch.
2. Add the file to your `<config>/custom_templates`-folder.
3. Reload your configuration from within the developer tools in Home Assistant.
4. The template should now be ready for use.

</details>

<details>

<summary>With HACS</summary>

<br>

1. Search for "*Jinja Speech Helpers German*" in HACS.
2. Download the repository via HACS.
3. The template should now be ready for use.

You can also use this button to add the repository to your Home Assistant instance via HACS:

[![Open your Home Assistant instance and open a repository inside the Home Assistant Community Store.](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=Nuhser&repository=jinja-speech-helpers-german&category=Template)

</details>

## Documentation

### plural

Looks at the given quantity and depending on it, returns `word_singular` or `word_plural`.

#### Syntax

`plural(word_singular, word_plural, quantity, show_quantity=true)`

| Param | Type | Explanation |
| :----- | :---- | :----------- |
| `word_singular` | string | Will be used as the output if `quantity` **is 1**. |
| `word_plural` | string | Will be used as the output if `quantity` **is 0** or **above 1**. |
| `quantity` | number | The quantity of the word that should be pluralized. |
| `show_quantity` | boolean | Will add the given quantity to the output infront of the word. (*Default:* `true`) |

#### Examples

**Input**

```jinja2
{% from 'speech_helpers_german.jinja' import plural %}
Du hast {{ plural('Apfel', 'Äpfel', 3) }}.
Es gibt ein Problem mit {{ plural('der Lampe', 'den Lampen', 1, false) }}.
```

**Output**

```text
Du hast 3 Äpfel.
Es gibt ein Problem mit der lampe.
```

*********************

### plural_verb

Looks at the given quantity and depending on it, returns the words "*ist*" or "*sind*".

#### Syntax

`plural_verb(quantity)`

| Param | Type | Explanation |
| :----- | :---- | :----------- |
| `quantity` | number | If it is **1** "*ist*" will be returned. Otherwise, "*sind*" will be returned. |

#### Examples

**Input**

```jinja2
{% from 'speech_helpers_german.jinja' import plural_verb %}
Folgende Tür(en) {{ plural_verb(2) }} offen: ...
```

**Output**

```text
Folgende Tür(en) sind offen: ...
```

*********************

### plural_word_and_verb

Works like a combination of `plural` and `plural_verb`. It looks at the given quantity and uses the corresponding word but also adds the correct verb before or after the word.

#### Syntax

`plural_word_and_verb(word_singular, word_plural, quantity, show_quantity=true, verb_first=true)`

| Param | Type | Explanation |
| :----- | :---- | :----------- |
| `word_singular` | string | Will be used as the output if `quantity` **is 1**. |
| `word_plural` | string | Will be used as the output if `quantity` **is 0** or **above 1**. |
| `quantity` | number | The quantity of the word that should be pluralized. Also decides if the verb should be "*ist*" (singular) or "*sind*" (plural). |
| `show_quantity` | boolean | Will add the given quantity to the output infront of the word. (*Default:* `true`) |
| `verb_first` | boolean | Decides if the verb should be added before the number and word or after (see examples below). (*Default:* `true`) |

#### Examples

**Input**

```jinja2
{% from 'speech_helpers_german.jinja' import plural_word_and_verb %}
Es {{ plural_word_and_verb('Tür', 'Türen', 0) }} offen.
{{ plural_word_and_verb('Tür', 'Türen', 1, true, false) }} gekippt.
{{ plural_word_and_verb('Ein Licht', 'Mehrere Lichter', 5, false, false) }} angeschaltet.
```

**Output**

```text
Es sind 0 Türen offen.
1 Tür ist gekippt.
Mehrere Lichter sind angeschlatet.
```

*********************

### array_to_separated_list

Converts an array of strings into a single string with (comma-)separated elements. By default the separator used will be "*,* " and the last separator will be replaced by " *und* ". You can also convert the elements from the array into title-case.

#### Syntax

`array_to_separated_list(array, separator=', ', replace_last_separator=true, convert_to_title=false)`

| Param | Type | Explanation |
| :----- | :---- | :----------- |
| array | list of strings | The array of strings that should be converted. |
| separator | string | The string that is added between the elements during convertion. (*Default:* `', '`) |
| replace_last_separator | boolean | If `true`, the last separator will be replaced by " *und* ". (*Default:* `true`) |
| convert_to_title | boolean | If `true`, the elements of the array will be converted into title-case. (*Default:* `false`) |

#### Examples

**Input**

```jinja2
{% from 'speech_helpers_german.jinja' import array_to_separated_list %}
{{ array_to_separated_list(['foo', 'bar', 'test']) }}
{{ array_to_separated_list(['foo', 'bar', 'test'], convert_to_title=true) }}
{{ array_to_separated_list(['foo', 'bar', 'test'], ' & ', false) }}
{{ array_to_separated_list(['foo', 'bar', 'test'], replace_last_separator=false) }}
```

**Output**

```text
foo, bar und test
Foo, Bar und Test
foo & bar & test
foo, bar, test
```

*********************

### array_to_bullet_points_list

Converts an array of strings into a bullet point list. At the end of each bullet point, an `<br/>` is added. This will be converted into line breaks when using the output for [phone notifications](https://companion.home-assistant.io/docs/notifications/notifications-basic/) or [HTML5 Push Notifications](https://www.home-assistant.io/integrations/html5).

#### Syntax

`array_to_bullet_points_list(array, bullet_symbol='- ')`

| Param | Type | Explanation |
| :----- | :---- | :----------- |
| array | list of strings | The array of strings that should be converted. |
| bullet_symbol | string | The string that should be added at the beginning of every line as the bullet point symbol. (*Default:* `'- '`) |

#### Examples

**Input**

```jinja2
{% from 'speech_helpers_german.jinja' import array_to_bullet_points_list %}
{{ array_to_bullet_points_list(['foo', 'bar', 'test']) }}

{{ array_to_bullet_points_list(['foo', 'bar', 'test'], '‣ ') }}
```

**Output Raw**

```text
-  foo<br/>-  bar<br/>-  test<br/>

‣  foo<br/>‣  bar<br/>‣  test<br/>
```

**Output in Notification**

```text
-  foo
-  bar
-  test

‣  foo
‣  bar
‣  test
```
