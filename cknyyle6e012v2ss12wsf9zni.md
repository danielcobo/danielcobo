## Get user's preferred languages with JavaScript

If you have different language settings in your website or application wouldn't it be great if you could automatically set the language the user prefers? Doing so may be easer than you think. All you have to do is use `navigator.language` or better, `navigator.languages`.

[Skip to solution](#solution)

## What's the difference between `navigator.language` and `navigator.languages`?

They're basically the same, except [`navigator.languages`][1] is newer and should return the whole list of user languages by order of preference. This is great in case you don't have a translation in their top language, but perhaps you do for a fallback. 

Speaking of fallbacks since `navigator.languages` implementation is experimental at the time of writing, it is wise to use [`navigator.language`][2] as a fallback value. Just remember to wrap it in an `Array` for consistency.

<h2 id="solution">Solution</h2>

%[https://codepen.io/DanielCobo/pen/KKaJYEx]

1. Using `navigator.languages` (`[navigator.language]` as fallback) we get the preferred language list
2. The list is based on ISO 639-1 and may indicate country by the alpha 2 code of ISO 3166. You probably won't have separate localisations based on country. That is why we filter out unique language codes.
3. Lastly we convert the language codes to more human-friendly english names of the languages. 

[1]: https://developer.mozilla.org/en-US/docs/Web/API/NavigatorLanguage/languages
[2]: https://developer.mozilla.org/en-US/docs/Web/API/NavigatorLanguage/language