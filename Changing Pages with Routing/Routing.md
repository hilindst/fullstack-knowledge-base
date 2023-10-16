# Routing

import {HttpModule} from @angular/http
import {Routes, RouterModule} from @angular/router to app.module.ts
adding 
    const appRoutes: Routes =[
      {path: 'users', component: UsersComponent }
      {path: '', component: HomeComponent }
    ]

    imports: [
      HttpModule,
      RouterModule.forRoot(appRoutes)
    ]

    and in app html to show where angular should load currently selected
        <router-outlet></router-outlet>

## routing links
  link <a routerLink="/">Home</a>

  or property binding
    <a [routerLink]="['/users']">Users</a>

### styling links
telling angular that this routerlinkactive class is exact so that other links arent "active" styled

    <li role="presentation" routerLinkActive="active" [routerLinkActiveOptions]="{exact: true}">
    <a routerLink="/">Home</a></li>

### navigating programmatically
    constructor(private router: Router) {}

    onLoadServers(){
      this.router.navigate(['/servers']);
    }
    
### Route Parameters

    const appRoutes: Routes =[
      {path: 'users', component: UsersComponent }
      {path: 'users/:id', component: UserComponent }
      {path: '', component: HomeComponent }
    ]

    ngOnInit(){
      this.user={
          id: this.route.snapshot.params['id'],
          name: this.route.snapshot.params['name']
        };
      }
    
in html {{user.id}}


subsequent changes with observable
 ngOnInit(){
      this.user={
          id: this.route.snapshot.params['id'],
          name: this.route.snapshot.params['name']
        };
        this.route.params.subscribe(
          (params: Params) => {
            this.user.id = params['id'],
            this.user.name = params['name'];
          }
        );
      }

### Query Parameters

    [queryParams]="{allowEdit: '1'}"
    fragment="loading"

    OnLoadServer(id: number){
      this.router.navigate(['/servers', id, 'edit'], {queryParams: {allowEdit: '1'}, fragment: 'loading'});
    }
    
### Retrieving query params    

## Nested Routes
    const appRoutes: Routes =[
        {path: 'users', component: UsersComponent, children:[ 
          {path: ':id/:name', component: UserComponent}
         ] 
        },
        {path: '', component: HomeComponent }
    ]

## Outsourcing Route Configuration
building a app-routing.module.ts to hold all the 
    const appRoutes: Routes = [];


### Guards
  ActivatedRouteSnapshot, RouterStateSnapshot,
  canActivate & canActivateChildRoute,
  canDeactivate,
  observable: boolean | promise:boolean | boolean;




