# Pipes

transforming output

    {{ server.instanceType | uppercase }} |
    {{ server.started | date: 'fullDate' | uppercase }}

  reads  SMALL | Sunday, August 8, 1920

: parameters

usually left to right operation
  order is important sometimes

## Custom Pipe
  shorten.pipe.ts

  @Pipe({
    name: 'shorten'
  })
  export class ShortenPipe implements PipeTransform {
    transform(value: any) {
      if(value.length > 10){
      return value.substr(0, 10) + '...';  //first 10 characters
      }
        return value;
    }
  }

  in app module add new custom Pipe to declarations
  in html template 
      {{ server.name | shorten }}

## Allow user to set limit
  export class ShortenPipe implements PipeTransform {
    transform(value: any, limit: number) {
      if(value.length > limit){
      return value.substr(0, limit) + '...';  //first 10 characters
      }
        return value;
    }
  }


    in html {{ server.name | shorten:15 }}


## Filter with Pipes 
  compoonent.ts
    filteredStatus = '';

new pipe    ng g p filter

    filter.pipe.ts  add to app module also

    app template
      <input type="text" [(ngModel)]="filteredStatus">
      <hr>
      <ul class="list-group">
        <li classe="list-group-item"
        *ngFor="let server of servers | filter: filteredStatus:'status'"></li>

    transform(value: any, filterString: string, propName: string): any {
      if(value.length === 0 || filterString === ''){
        return value;
      }
      const resultArray = [];
      for (const item of value) {
        if(item[propName] === filterString) {
          resultArray.push(item);
        }
      }
      return resultArray;
    }

  ### recalculate pipe when data changes
  @pipe({
      name : 'filter';
      pure: false;
        })