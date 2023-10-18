#Observables

Observable and Observer
with multiple data packages over a timeline

### Observer
1) handling data
2) handling error
3) handling completion

All these "Events" are asynchronous waiting for user input usually



### package rxjs install
In order to follow along smoothly with the course examples, make sure you install RxJS v6 by running

    npm install --save rxjs@6

In addition, also install the rxjs-compat package:

    npm install --save rxjs-compat


### Interval

period in milliseconds
    interval( period: 1000).subscribe( next: count => {
        console.log(count);
    })

    will log incrementing values in the console log every 1000ms


store subscriptions and delete subscriptions to prevent memory leaks

    export class HomeComponent implements OnInit, OnDestroy {
      private firstObsSubscription: Subscription;
    NgOnInit(){ 
      this.firstObsScupbriction = interval( period: 1000).subscribe( next: count => {
        console.log(count);
      })
    }

      //stores subscription//

    ngOnDestroy(): void {
      this.firstObsSubscription.unsubscribe();
    }
   }

   ### Custom Observable same interval action
     export class HomeComponent implements OnInit, OnDestroy {
      private firstObsSubscription: Subscription;
    NgOnInit(){ 
     const customIntervalObservable = Observable.create( observer => {
      let count = 0;
        setInterval(handler: () => {
          observer.next(count);
          count++;
        }, timeout: 1000);
     });
    }

    customIntervalObservable.subscribe(data => {
      console.log(data);
    });

    ngOnDestroy(): void {
      this.firstObsSubscription.unsubscribe();
    }
   }

observer.next
observer.error
observer.complete

## Error Handling

   export class HomeComponent implements OnInit, OnDestroy {
      private firstObsSubscription: Subscription;
    NgOnInit(){ 
     const customIntervalObservable = Observable.create( observer => {
      let count = 0;
        setInterval(handler: () => {
          observer.next(count);
          if (count > 3) {
              observer.error(new Error (message: 'Count is greater than3!')):
          }
          count++;
        }, timeout: 1000);
     });
    }

    this.firstObsSubscription = customIntervalObservable.subscribe(data => {
      console.log(data);
    }, error => {
      console.log(error);
      alert(error.message);
    }
    );

    ngOnDestroy(): void {
      this.firstObsSubscription.unsubscribe();
    }
   }

## Handling Completion

export class HomeComponent implements OnInit, OnDestroy {
      private firstObsSubscription: Subscription;
    NgOnInit(){ 
     const customIntervalObservable = Observable.create( observer => {
      let count = 0;
        setInterval(handler: () => {
          observer.next(count);
          if (count === 2) {
            observer.complete();
          }
          if (count > 3) {
              observer.error(new Error (message: 'Count is greater than3!')):
          }
          count++;
        }, timeout: 1000);
     });
    }

    this.firstObsSubscription = customIntervalObservable.subscribe(data => {
      console.log(data);
    }, error => {
      console.log(error);
      alert(error.message);
    }, () => {
      console.log('Complete');
    }
    );

    ngOnDestroy(): void {
      this.firstObsSubscription.unsubscribe();
    }
   }

handled errors will not trigger completion
  an error cancels an observable

## Operators
observable data points that reach operators and then the observer subscribes to the operator
operators are loaded from rjxs
like pipe
map
filter


    this.firstObsSubscription = customIntervalObservable.pipe(filter(data => {
      return data > 0;
    }), map(project: (data: number) => {
      return 'Round: ' + (data + 1);
    })).subscribe(data => {
      console.log(data);
    }, error => {
      console.log(error);
      alert(error.message);
    }, () => {
      console.log('Complete');
    }
    );
