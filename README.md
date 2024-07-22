# goit-js-hw-10
### Zadanie domowe nr 10

_Trzy czwarte kursu JavaScript już za nami!_ 💪

_Podsumujmy, czego nauczyliśmy się w module 10._

_Sprawdź się! Na chwilę obecną Ty:_

* _rozumiesz różnicę między kodem synchronicznym i asynchronicznym;_
* _wiesz, jak odłożyć wywołanie funkcji na określony czas za pomocą time-outów i interwałów;_
* _rozumiesz, czym są obietnice i jak działają;_
* _znasz podstawowe metody klasy Promise._
_Czas wykorzystać tę wiedzę w praktyce, pracując nad licznikiem czasu i generatorem obietnic!_

__Zadanie domowe nr 10__

* Utwórz repozytorium `goit-js-hw-10`.
* Zbuduj projekt za pomocą [Vite](https://vitejs.dev/). Przygotowaliśmy dla Ciebie [gotową kompilację](https://github.com/goitacademy/vanilla-app-template) ze wszystkimi dodatkowymi ustawieniami projektu i zalecamy jej użycie.
* Przeczytaj zadanie i wykonaj go w edytorze kodu.
* Upewnij się, że kod jest sformatowany przy użyciu `Prettier` i że po otwarciu strony zadania na żywo w konsoli nie ma żadnych błędów ani ostrzeżeń.
* Prześlij zadanie domowe do sprawdzenia.


__Format zadania domowego:__ Zadanie domowe zawiera dwa linki — do plików źródłowych i strony roboczej na `GitHub Pages`.



Struktura folderów i plików w folderze `src` powinna wyglądać następująco:

![Struktura folderów i plików w folderze src](https://filedn.eu/lPq6O1K7j8DR1n7JwTuYjYz/img/warsztaty/e600d7ff-053c-42b6-914d-fb182620b25cimage-zda10.png)





__Zadanie 1. Licznik czasu__

Wykonaj to zadanie w plikach `01-timer.html` і `01-timer.js`. Napisz skrypt timera, który odlicza czas do określonej daty. Taki licznik może być używany na blogach, w sklepach internetowych, na stronach rejestracji na wydarzenia, podczas prac technicznych itp. Obejrzyj film demonstracyjny licznika.


[![Licznik czasu](https://filedn.eu/lPq6O1K7j8DR1n7JwTuYjYz/img/warsztaty/video10.jpg)](https://goitlmsstorage.b-cdn.net/9be6b073-16fc-49be-a308-da36e9e1c38a16.mp4)


__Elementy interfejsu__

Dodaj do pliku HTML znaczniki licznika czasu, pola wyboru daty zakończenia i przycisk, który powinien uruchamiać licznik czasu po kliknięciu. Dodaj wygląd elementów interfejsu zgodnie z układem.


```html
<input type="text" id="datetime-picker" />
<button type="button" data-start>Start</button>

<div class="timer">
  <div class="field">
    <span class="value" data-days>00</span>
    <span class="label">Days</span>
  </div>
  <div class="field">
    <span class="value" data-hours>00</span>
    <span class="label">Hours</span>
  </div>
  <div class="field">
    <span class="value" data-minutes>00</span>
    <span class="label">Minutes</span>
  </div>
  <div class="field">
    <span class="value" data-seconds>00</span>
    <span class="label">Seconds</span>
  </div>
</div>
```


__Biblioteka__ `flatpickr`

Użyj biblioteki [flatpickr](https://flatpickr.js.org/), aby umożliwić użytkownikowi wybór daty i godziny zakończenia w różnych przeglądarkach w jednym elemencie interfejsu użytkownika. Aby dołączyć kod CSS biblioteki do swojego projektu, musisz dodać jeszcze jeden import, oprócz tego opisanego w dokumentacji.

```javascript
// Opisany w dokumentacji
import flatpickr from "flatpickr";
// Dodatkowy import stylów
import "flatpickr/dist/flatpickr.min.css";
```

Biblioteka oczekuje inicjalizacji na elemencie `input[type="text"]`, więc dodaliśmy do dokumentu HTML pole `input#datetime-picker`.

```html
<input type="text" id="datetime-picker" />
```

Jako drugi argument funkcji `flatpickr(selector, options)` można przekazać opcjonalny obiekt opcji. Przygotowaliśmy już dla Ciebie obiekt potrzebny do wykonania tego zadania. Dowiedz się, za co odpowiadają poszczególne właściwości w [dokumentacji «Options»](https://flatpickr.js.org/options/) і wykorzystaj je w swoim kodzie.


```javascript
const options = {
  enableTime: true,
  time_24hr: true,
  defaultDate: new Date(),
  minuteIncrement: 1,
  onClose(selectedDates) {
    console.log(selectedDates[0]);
  },
};
```

__Wybór daty__

Metoda `onClose()` z obiektu parametru jest wywoływana za każdym razem, gdy zamykany jest element interfejsu, który tworzy `flatpickr`. Właśnie w tej metodzie należy przetwarzać datę wybraną przez użytkownika. Parametr `selectedDate`s jest tablicą wybranych dat, więc bierzemy pierwszy element `selectedDates[0]`.



Będziesz potrzebować tej wybranej daty w kodzie poza metodą `onClose()`. Dlatego zadeklaruj zmienną poza metodą `let`, na przykład `userSelectedDate`, a po sprawdzeniu poprawności w metodzie `onClose()` dla przeszłości/przyszłości, zapisz wybraną datę do tej zmiennej `let`.



* Jeśli użytkownik wybrał datę w przeszłości, wyświetl `window.alert()` z tekstem `"Please choose a date in the future"` і uczyń przycisk «Start» nieaktywnym.
* Jeśli użytkownik wybrał prawidłową datę (w przyszłości), przycisk «Start» staje się aktywny.
* Przycisk «Start» powinien być nieaktywny, dopóki użytkownik nie wybierze daty w przyszłości. Zwróć uwagę, że jeśli użytkownik wybierze prawidłową datę, nie uruchomi licznika czasu, a następnie wybierze nieprawidłową datę, to po odblokowaniu przycisk powinien ponownie stać się nieaktywny.
* Naciśnięcie przycisku «Start» rozpoczyna odliczanie do wybranej daty od momentu jego naciśnięcia.


__Odliczanie czasu__

Po naciśnięciu przycisku «Start» skrypt powinien co sekundę obliczać ilość czasu pozostałego do określonej daty i aktualizować interfejs licznika czasu, wyświetlając cztery cyfry: dni, godziny, minuty i sekundy w formacie `xx:xx:xx:xx`.

* Liczba dni może składać się z więcej niż dwóch cyfr.
* Licznik powinien zatrzymać się, gdy osiągnie datę końcową, tj. pozostały czas wynosi zero `00:00:00:00`.


NIE KOMPLIKUJMY SPRAWY Jeśli licznik czasu jest uruchomiony, aby wybrać nową datę i uruchomić go ponownie, należy odświeżyć stronę.

Aby obliczyć wartości, użyj gotowej funkcji `convertMs`, gdzie `ms` — to różnica między datą końcową a bieżącą datą w milisekundach.


```javascript
function convertMs(ms) {
  // Number of milliseconds per unit of time
  const second = 1000;
  const minute = second * 60;
  const hour = minute * 60;
  const day = hour * 24;

  // Remaining days
  const days = Math.floor(ms / day);
  // Remaining hours
  const hours = Math.floor((ms % day) / hour);
  // Remaining minutes
  const minutes = Math.floor(((ms % day) % hour) / minute);
  // Remaining seconds
  const seconds = Math.floor((((ms % day) % hour) % minute) / second);

  return { days, hours, minutes, seconds };
}

console.log(convertMs(2000)); // {days: 0, hours: 0, minutes: 0, seconds: 2}
console.log(convertMs(140000)); // {days: 0, hours: 0, minutes: 2, seconds: 20}
console.log(convertMs(24140000)); // {days: 0, hours: 6 minutes: 42, seconds: 20}
```


__Formatowanie czasu__

Funkcja `convertMs()` zwraca obiekt z obliczonym czasem pozostałym do daty końcowej. Zwróć uwagę, że nie formatuje ona wyniku. Oznacza to, że jeśli pozostały 4 minuty lub jakikolwiek inny składnik czasu, funkcja zwróci `4`, а nie `04`. W interfejsie licznika czasu należy dodawać `0`, jeśli liczba jest mniejsza niż dwa znaki. Napisz funkcję, na przykład `addLeadingZero(value)`, która używa metody łańcuchowej `[padStart()](<https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/padStart>)` і formatuje wartość przed narysowaniem interfejsu.



__Biblioteka powiadomień__

Aby wyświetlać powiadomienia użytkownikowi, zamiast `window.alert()` użyj biblioteki [iziToast](https://izitoast.marcelodolza.com/).

Na co mentor zwróci uwagę podczas sprawdzania:

* Podłączone biblioteki flatpickr i iziToast.
* Gdy strona jest ładowana po raz pierwszy, przycisk Start jest nieaktywny.
* Po kliknięciu wejścia otwiera się kalendarz, w którym można wybrać datę.
* Po wybraniu daty z przeszłości przycisk Start staje się nieaktywny i pojawia się komunikat z tekstem `"Please choose a date in the future"`.
* Po wybraniu daty w przyszłości przycisk Start staje się aktywny.
* Po kliknięciu przycisku Start staje się on nieaktywny, a na stronie wyświetlany jest czas pozostały do wybranej daty w formacie `xx:xx:xx:xx` i rozpoczyna się odliczanie do wybranej daty.
* Interfejs jest aktualizowany co sekundę i pokazuje zaktualizowane dane dotyczące pozostałego czasu.
* Timer zatrzymuje się, gdy osiągnie datę końcową, tj. pozostały czas wynosi zero, a interfejs wygląda następująco: `00:00:00:00`.
* Czas w interfejsie jest sformatowany, a jeśli zawiera mniej niż dwa znaki, na początku liczby dodawane jest 0.


__Zadanie 2. Generator obietnic__

Wykonaj to zadanie w plikach `2-snackbar.html` і `02-snackbar.js`. Obejrzyj film demonstracyjny generatora obietnic.

<video src="https://goitlmsstorage.b-cdn.net/64d30329-b2a2-4f96-922a-f3fb3ce07a2417.mp4" preload="auto" controls="" style="width: 100%; height: auto;"></video>

Dodaj do pliku HTML znaczniki formularza. Formularz składa się z pola wejściowego do wprowadzenia wartości opóźnienia w milisekundach, dwóch przycisków radiowych określających sposób wykonania obietnicy oraz przycisku typu `submit`, którego kliknięcie spowoduje utworzenie obietnicy.


```html
<form class="form">
  <label>
    Delay (ms)
    <input type="number" name="delay" required />
  </label>

  <fieldset>
    <legend>State</legend>
    <label>
      <input type="radio" name="state" value="fulfilled" required />
      Fulfilled
    </label>
    <label>
      <input type="radio" name="state" value="rejected" required />
      Rejected
    </label>
  </fieldset>

  <button type="submit">Create notification</button>
</form>
```


Napisz skrypt, który po przesłaniu formularza utworzy obietnicę. W połowie wywołania zwrotnego tej obietnicy po upływie określonej przez użytkownika liczby milisekund obietnica powinna zostać wykonana (jeśli "fulfilled") lub odrzucona (jeśli "rejected"), w zależności od opcji wybranej za pomocą przycisków radiowych. Wartością obietnicy przekazywanej jako argument do metod resolve/reject powinna być wartość opóźnienia w milisekundach.



Utworzoną obietnicę należy przetworzyć w odpowiednich metodach dla powodzenia/niepowodzenia.



Jeśli obietnica zostanie wykonana pomyślnie, wyświetl w konsoli następujący wiersz, gdzie `delay` jest wartością opóźnienia w milisekundach.


```javascript
`✅ Fulfilled promise in ${delay}ms`
```


Jeśli obietnica zostanie odrzucona, wyświetl w konsoli następujący wiersz, gdzie `delay` jest wartością opóźnienia obietnicy w milisekundach.


```javascript
`❌ Rejected promise in ${delay}ms`
```


__Biblioteka powiadomień__

Do wyświetlania powiadomień zamiast `console.log()` użyj biblioteki [iziToast](https://izitoast.marcelodolza.com/). Aby dołączyć kod CSS biblioteki do swojego projektu, musisz dodać kolejny import, oprócz tego opisanego w dokumentacji.

```javascript
// Opisany w dokumentacji
import iziToast from "izitoast";
// Kolejny import stylów
import "izitoast/dist/css/iziToast.min.css";
```

Na co mentor zwróci uwagę podczas sprawdzania:

* Podłączona biblioteka iziToast.
* Po wybraniu stanu za pomocą przycisków radiowych i kliknięciu przycisku Create notification zostanie wyświetlony komunikat odpowiadający wybranemu stanowi stylu z opóźnieniem wynoszącym liczbę milisekund przesłanych do wejścia.
* Wyświetlany komunikat zawiera typ wybranego stanu i liczbę milisekund zgodnie z szablonem w warunku.

https://lukasz-sklad.github.io/goit-js-hw-10/