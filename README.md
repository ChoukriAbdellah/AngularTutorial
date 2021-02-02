# AngularTuto
> Some importants informations  from [AngularTutorial](https://angular.io/tutorial)


- Error handling
Things go wrong, especially when you're getting data from a remote server. The `HeroService.getHeroes()`  method should catch errors and do something appropriate.
To catch errors, you "pipe" the observable result from `http.get()`  through an RxJS `catchError()` operator.
Import the catchError symbol from rxjs/operators, along with some other operators you'll need later.
 ```javascript=
 //src/app/hero.service.ts
 getHeroes(): Observable<Hero[]> {
  return this.http.get<Hero[]>(this.heroesUrl)
    .pipe(
      catchError(this.handleError<Hero[]>('getHeroes', []))
    );
    }
```
The `catchError()` operator intercepts an Observable **that failed.** The operator then passes the error to the error handling function. The following `handleError()` method reports the error and then returns an innocuous result so that the application keeps working.
**handleError**
The following `handleError()`  will be shared by many HeroService methods so it's generalized to meet their different needs.

Instead of handling the error directly, it returns an error handler function to catchError that it has configured with both the name of the operation that failed and a safe return value.

```javascript=
/**
 * Handle Http operation that failed.
 * Let the app continue.
 * @param operation - name of the operation that failed
 * @param result - optional value to return as the observable result
 */
private handleError<T>(operation = 'operation', result?: T) {
  return (error: any): Observable<T> => {

    // TODO: send the error to remote logging infrastructure
    console.error(error); // log to console instead

    // TODO: better job of transforming error for user consumption
    this.log(`${operation} failed: ${error.message}`);

    // Let the app keep running by returning an empty result.
    return of(result as T);
  };
}
```
:information_source:  Trim method
>Remove whitespace from both sides of a string.

- AsyncPipe
The ***ngFor** repeats hero objects. Notice that the ***ngFor** iterates over a list called heroes$, not heroes. The $ is a convention that indicates heroes$ is an Observable, not an array.
```html
<li *ngFor="let hero of heroes$ | async" >
```
- The searchTerms RxJS subject
The searchTerms property is an RxJS Subject.
```javascript=
private searchTerms = new Subject<string>();

// Push a search term into the observable stream.
search(term: string): void {
  this.searchTerms.next(term);
}
```
A Subject is both a source of observable values and an Observable itself. You can subscribe to a Subject as you would any Observable. You can also push values into that Observable by calling its next(value) method as the search() method does.
The event binding to the textbox's input event calls the search() method
```html
<input #searchBox id="search-box" (input)="search(searchBox.value)" />
```
>Every time the user types in the textbox, the binding calls search() with the textbox value, a "search term". The searchTerms becomes an Observable emitting a steady stream of search terms.

#### angular-ng-autocomplete :mag:
> For to give the best experience for the user I prefer use angular-ng-autocomplite.  

#####  Install `angular-ng-autocomplete`:

#### NPM
```shell
npm i angular-ng-autocomplete
```
###  Import the AutocompleteLibModule:
```js
import {AutocompleteLibModule} from 'angular-ng-autocomplete';

@NgModule({
  declarations: [AppComponent],
  imports: [AutocompleteLibModule],
  bootstrap: [AppComponent]
})
export class AppModule {}
```
### Usage sample

```html
<div class="ng-autocomplete">
<ng-autocomplete 
  [data]="data"
  [searchKeyword]="keyword"
  (selected)='selectEvent($event)'
  (inputChanged)='onChangeSearch($event)'
  (inputFocused)='onFocused($event)'
  [itemTemplate]="itemTemplate"
  [notFoundTemplate]="notFoundTemplate">                                 
</ng-autocomplete>

<ng-template #itemTemplate let-item>
<a [innerHTML]="item.name"></a>
</ng-template>

<ng-template #notFoundTemplate let-notFound>
<div [innerHTML]="notFound"></div>
</ng-template>
</div>

```
```javascript

class TestComponent {
  keyword = 'name';
  data = [
     {
       id: 1,
       name: 'Usa'
     },
     {
       id: 2,
       name: 'England'
     }
  ];


  selectEvent(item) {
    // do something with selected item
  }

  onChangeSearch(val: string) {
    // fetch remote data from here
    // And reassign the 'data' which is binded to 'data' property.
  }
  
  onFocused(e){
    // do something when input is focused
  }
}
```

##  Références
[AngularTutorial](https://angular.io/tutorial)
[angular-ng-autocomplete](https://github.com/gmerabishvili/angular-ng-autocomplete/blob/master/README.md)