Флаги-эмодзи дают много возможностей для визуального отображения данных о странах. Если разобраться, как они работают и как создавать их программно, можно сделать интерфейс более простым и привлекательным для пользователей.

# Флаги и их структура

Они как и все остальные эмодзи, формируются с использованием символов Unicode. Каждый флаг-эмодзи состоит из двух символов. Они находятся в диапазоне от A-Z и предназначены для обозначения двухбуквенных кодов стран. Например, флаг Германии 🇩🇪 состоит из комбинации `U+1F1E9(D)` и `U+1F1EA(E)`.

![Структура флагов](https://cdn.sparrowcode.io/tutorials/emoji-flags/struct-flags.png)

# Получаем флаги через код страны

Зная о кодах стран и их эмодзи напишем простую функцию, которая автоматизирует этот процесс. Она может динамически генерировать флаги на основе введенных кодов стран.

```swift
func emojiFlag(countryCode: String) -> String? {
   guardcountryCode.count == 2 else {
   returnnil
}
   // https://en.wikipedia.org/wiki/Regional_indicator_symbol
   let regionalIndicatorStartIndex: UInt32 = 0x1F1E6
   let alphabetOffset = UnicodeScalar(unicodeScalarLiteral: "A").value

   return String(countryCode
   .uppercased()
   .unicodeScalars
   .compactMap { UnicodeScalar(
   regionalIndicatorStartIndex + ($0.value - alphabetOffset)
   .map { Character($0) }
   )
}

emojiFlag(countryCode: "CA") // "🇨🇦"
```

Это ускорить процесс добавления флагов в свое приложение, значительно ускоряя и упрощая процесс.

# Флаги внутри страны

На момент написания этой статьи, возможно добавление флагов подразделений страны, например, флаги Англии, Шотландии и Уэльса, что открывает еще больше возможностей.

![Флаги внутри страны](https://cdn.sparrowcode.io/tutorials/emoji-flags/country-flags.png)

Они используют другую последовательность символов, которая начинается с черного флага `U+1F3F4(🏴)` и заканчивается специальным кодом символа отмены `U+E007F`.


Используем предыдущую функцию, чтобы добавить вариант флагов внутри страны:

```swift
func emojiFlag(subdivision: String) -> String? {
   guard let blackFlag = Unicode.Scalar(0x1F3F4),
   let cancelTag = Unicode.Scalar(0xE007F) else {
   return nil
}
   // https://en.wikipedia.org/wiki/Tags_(Unicode_block)
   let tagLetterOffset: UInt32 = 0xE0061
   let alphabetOffset = UnicodeScalar(unicodeScalarLiteral: "a").value
   return String(Character(blackFlag)) + String(subdivision
   .lowercased()
   .unicodeScalars
   .compactMap { Unicode.Scalar(
   tagLetterOffset + ($0.value - alphabetOffset)
   )}
   .map { Character($0) }
   ) + String(Character(cancelTag))
}

emojiFlag(subdivision: "GBENG") // "🏴󠁧󠁢󠁥󠁮󠁧󠁿"
emojiFlag(subdivision: "GBSCT") // "🏴󠁧󠁢󠁳󠁣󠁴󠁿"
emojiFlag(subdivision: "GBWLS") // "🏴󠁧󠁢󠁷󠁬󠁳󠁿"
```

# Политика Unicode к новым флагам

Консорциум Unicode перестал принимать новые заявки на флаги. Это значит, что появление свежих эмодзи-флагов невозможно без официального признания страны от ISO. Если страна получает такой статус, её флаг-эмодзи появится в следующей версии Unicode автоматически.


