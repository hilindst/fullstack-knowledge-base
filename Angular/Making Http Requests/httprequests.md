# HTTP Requests

## Connecting Angular to a Database

not directly, too insecure since its a front end javascript app and everyone can read the code

sending http requests to and from a server(API)
  REST API 
  Json

### HTTP Request anatomy

URL (API Endpoint)
HTTP Verb (post, get, put,...)
Headers (Metadata)
Body 

### HTTP Request
using post and subscribe

    constructor(private http: HttpClient){}
    onCreatePost(postData: {title: string; content: string}){
      this.http.post('https://ng-database-test-default-rtdb.firebaseio.com/posts.json', postData
      ).subscribe(responseData => {
        console.log(responseData);
      });
    }

    in app module
      HttpClientModule from @angular/common/http into imports array
this subscription will clear itself by angular

.json is a requirement by firebase API

Angular HTTP Client converts string to JSON data which is what the API accepts

Network tab in developer tools
  to check network activity and http requests

### Getting Data

    private fetchPosts(){
      this.http.get('https://ng-database-test-default-rtdb.firebaseio.com/posts.json').subscribe(posts => {
        console.log(posts);
      });
    }

    call this.fetchPosts() in NG OnInit as well

    returns javascript object that needs to be transformed

    use map operator from rjxs

    private fetchPosts(){
      this.http.get('https://ng-database-test-default-rtdb.firebaseio.com/posts.json')
      .pipe(map((responseData: {[key:string]}):  => {
        const postsArray = [];
        for (const key in responseData) {
          if(responseData.hasOwnProperty(key)) {
          postsArray.push({...responseData[key], id: key });
          }
        }
        return postsArray;
      }))
      .subscribe(posts => {
        console.log(posts);
      });
    }

    console.log would return
      [{..}]
        content:blablabbh
        id: blabhjbh
        title: blahblahblah
        etc.

### to create TYPE of response data

create a post.model.ts
    export interface Post {
      title: string,
      id: string,
      content: string
    }


    private fetchPosts(){
      this.http.get('https://ng-database-test-default-rtdb.firebaseio.com/posts.json')
      .pipe(map((responseData: {[key:string]: Post }):  => {
        const postsArray: Post[] = [];
        for (const key in responseData) {
          if(responseData.hasOwnProperty(key)) {
          postsArray.push({...responseData[key], id: key });
          }
        }
        return postsArray;
      }))
      .subscribe(posts => {
        console.log(posts);
      });
    }

# OR because GET<> or  POST<> to set type on generic http request methods


    private fetchPosts(){
      this.http
      .get<{[key:string]: Post }>('https://ng-database-test-default-rtdb.firebaseio.com/posts.json')
      .pipe(map(responseData => {
        const postsArray: Post[] = [];
        for (const key in responseData) {
          if(responseData.hasOwnProperty(key)) {
          postsArray.push({...responseData[key], id: key });
          }
        }
        return postsArray;
      }))
      .subscribe(posts => {
        console.log(posts);
      });
    }

## Outputting Posts

  loadedPosts: Post[] = [];
    private fetchPosts(){
      this.http
      .get<{[key:string]: Post }>('https://ng-database-test-default-rtdb.firebaseio.com/posts.json')
      .pipe(map(responseData => {
        const postsArray: Post[] = [];
        for (const key in responseData) {
          if(responseData.hasOwnProperty(key)) {
          postsArray.push({...responseData[key], id: key });
          }
        }
        return postsArray;
      }))
      .subscribe(posts => {
        this.loadedPosts = posts;
      });
    }

    in html template

    <p *ngIf = "loadedPosts.length < 1">No Posts Available!</p>
    <ul class= "list-group" *ngIf="loadedPosts.length >= 1">
      <li class="list-group-item" *ngFor="let post of loadedPosts">
        <h3>{{ post.title }}</h3>
        <p>{{ post.content }}</p>
        </li>


## Loading Indicator
    isFetching = false;

     private fetchPosts(){
      this.isFetching = true;
      this.http
      .get<{[key:string]: Post }>('https://ng-database-test-default-rtdb.firebaseio.com/posts.json')
      .pipe(map(responseData => {
        const postsArray: Post[] = [];
        for (const key in responseData) {
          if(responseData.hasOwnProperty(key)) {
          postsArray.push({...responseData[key], id: key });
          }
        }
        return postsArray;
      }))
      .subscribe(posts => {
        this.isFetching = false;
        this.loadedPosts = posts;
      });
    }

    in html template
    <p *ngIf = "loadedPosts.length < 1 && !isFetching">No Posts Available!</p>
    <ul class= "list-group" *ngIf="loadedPosts.length >= 1 && !isFetching">
      <li class="list-group-item" *ngFor="let post of loadedPosts">
        <h3>{{ post.title }}</h3>
        <p>{{ post.content }}</p>
        </li>
        </ul>
        <p *ngIf="isFetching">Loading...</p>

## Using a service for HTTP Requests

    posts.service.ts

  constructor(private http: HttpClient){}

      createAndStorePost(title: string, content: string){
      const postData: Post = {title: title, content: content};
      this.http
      .post<{ name: string }>(
        'https://ng-database-test-default-rtdb.firebaseio.com/posts.json', 
        postData)
      .subscribe(responseData => {
        console.log(responseData);
      });
    }
      
    fetchPosts(){
      this.http
      .get<{[key:string]: Post }>('https://ng-database-test-default-rtdb.firebaseio.com/posts.json')
      .pipe(
        map(responseData => {
        const postsArray: Post[] = [];
        for (const key in responseData) {
          if(responseData.hasOwnProperty(key)) {
          postsArray.push({...responseData[key], id: key });
          }
        }
        return postsArray;
      }))
      .subscribe(posts => {
      });
    }
    

    inject this service into app.component.ts
      onCreatePost(postData: Post){
        this.postsService.createAndStorePost(postData.title, postData.content);
      }

      onFetchPosts(){
        this.postsService.fetchPosts();
      }

      ngOnInit() {
        this.postsService.fetchPosts();
      }

### Subject would be a good option for a service that is called from multiple components
but for a simpler app, RETURN can be called instead on the service and subsribe moved to app component.ts

    fetchPosts(){
        return this.http
          .get<{[key:string]: Post }>('https://ng-database-test-default-rtdb.firebaseio.com/posts.json')
          .pipe(
            map(responseData => {
              const postsArray: Post[] = [];
              for (const key in responseData) {
                if(responseData.hasOwnProperty(key)) {
                postsArray.push({...responseData[key], id: key });
                }
              }
              return postsArray;
            }))
        }


app.componenet.ts  Result handling in component, but the heavyduty lifting is in a service
    ngOnInit() {
      this.isFetching = true;
        this.postsService.fetchPosts().subscribe(posts => {
          this.isFetching = false;
          this.loadedPosts = posts;
        });
      }

      onFetchPosts(){
        this.isFetching = true;
          this.postsService.fetchPosts().subscribe(posts => {
            this.isFetching = false;
            this.loadedPosts = posts;
          });
      }

      onClearPosts(){

      }


in posts.service.ts
    deletePosts(){
      return this.http
      .delete('https://ng-database-test-default-rtdb.firebaseio.com/posts.json');
    }

and subscribe in app.component.ts and reset loaded array

    onClearPosts(){
      this.postsService.deletePosts().subscribe(() => {
        this.loadedPosts = [];  
      })
    }

## Handling Errors
depends on API

    error = null;

    onFetchPosts(){
        this.isFetching = true;
          this.postsService.fetchPosts().subscribe(posts => {
            this.isFetching = false;
            this.loadedPosts = posts;
          }, error => {
            this.error = error.message;
            console.log(error);
          });
      }

      same as ngOnInit(){}

in hhtml
    <p *ngIf="isFetching && !error">Loading...</p>
    <div class="alert alert-danger" *ngIf="error">
      <h1>An Error Occurred!</h1>
      <p>{{ error }}</p>
      </div>

### Subject Error handling wihtout a subscription in the service

    error = new Subject<string>();

     createAndStorePost(title: string, content: string){
      const postData: Post = {title: title, content: content};
      this.http
      .post<{ name: string }>(
        'https://ng-database-test-default-rtdb.firebaseio.com/posts.json', 
        postData)
      .subscribe(responseData => {
        console.log(responseData);
      }, error => {
        this.error.next(error.message)
      });
    }

component.ts
    error = null;
    private errorSub: Subscription;

    ngOnInit() {
      this.errorSub = this.postsService.error.subscribe(errorMessage => {
        this.error = errorMessage;
      });

      this.isFetching = true;
      this.postsService.fetchPosts().subscribe(posts => {
          this.isFetching = false;
          this.loadedPosts = posts;
        });
      }

    ngOnDestroy(){
      this.errorSub.unsubscribe();
    }

### CatchError Operator
in posts.service
    fetchPosts(){
        return this.http
          .get<{[key:string]: Post }>('https://ng-database-test-default-rtdb.firebaseio.com/posts.json')
          .pipe(
            map(responseData => {
              const postsArray: Post[] = [];
              for (const key in responseData) {
                if(responseData.hasOwnProperty(key)) {
                postsArray.push({...responseData[key], id: key });
                }
              }
              return postsArray;
            }),
            catchError(errorRes => {
              //send to analytics server
              return throwError(errorRes);
            }))
        }

## Setting Headers (Metadata)
key value pairs
import Headers

    fetchPosts(){
        return this.http
          .get<{[key:string]: Post }>(
            'https://ng-database-test-default-rtdb.firebaseio.com/posts.json',
            {
              headers: new HttpHeaders({ 'Custom-Header': 'Hello' })
            }
            )
          .pipe(
            map(responseData => {
              const postsArray: Post[] = [];
              for (const key in responseData) {
                if(responseData.hasOwnProperty(key)) {
                postsArray.push({...responseData[key], id: key });
                }
              }
              return postsArray;
            }),
            catchError(errorRes => {
              //send to analytics server
              return throwError(errorRes);
            }))
        }

## Query Params
import HTTpParams
fetchPosts(){
        return this.http
          .get<{[key:string]: Post }>(
            'https://ng-database-test-default-rtdb.firebaseio.com/posts.json',
            {
              headers: new HttpHeaders({ 'Custom-Header': 'Hello' }),
              params: new HttpParams().set('print', 'pretty')
            }
            )
          .pipe(
            map(responseData => {
              const postsArray: Post[] = [];
              for (const key in responseData) {
                if(responseData.hasOwnProperty(key)) {
                postsArray.push({...responseData[key], id: key });
                }
              }
              return postsArray;
            }),
            catchError(errorRes => {
              //send to analytics server
              return throwError(errorRes);
            }))
        }

Can add multiple params
    fetchPosts(){
      let searchParams = new HttpParams();
      searchParams = searchParams.append('print', 'pretty');
      return this.http
          .get<{[key:string]: Post }>(
            'https://ng-database-test-default-rtdb.firebaseio.com/posts.json',
            {
              headers: new HttpHeaders({ 'Custom-Header': 'Hello' }),
              params: searchParams
            }
            etc...
            )
    }

## Interceptors can use multiple at once

can modify, log request or response data
order of interceptors may matter for providers

create a inetercept service
    logging-interceptor.service.ts

