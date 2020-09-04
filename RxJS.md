The following example is from the article 
["5 Common Mistakes with RxJS"](https://blog.bitsrc.io/5-common-mistakes-with-rxjs-1b09d4c19387) by Giancarlo Buomprisco.

### Things to remember
> "One of the greatest things about RxJs is that combining operators and reusing their
> logic is an incredibly nice (and easy) way to build reusable bits of code. ... . RxJS streams are pipeable,
> which means they can be combined and extended and therefore reused." 
~Giancarlo Buomprisco.

> "...keep the logic within the subscription callbacks as small as possible"* 
~Giancarlo Buomprisco.

### Some rule of thumb:
* __Do not check whether a value is truthy within your subscription, you can easily handle it with the operator filter(Boolean)__
* __Do not transform data in your subscription__
* __Side effects: for example, showing/hiding a loading icon, can be done with the tap or/and the finalize operators__

#### Not so good practice

````javascript
class UsersDashboardComponent {
  users: User[];
  activeUsers: [];
  bannedUsers: [];
  constructor(private service: UsersService) {
     this.service.users$.subscribe(users => {
        if (users) {
          this.users = users;
          this.activeUsers = users.filter(user => user.active);
          this.bannedUsers = users.filter(user => user.banned);
        }
     });
  }
}
````

#### A better practice

````javascript
class UsersDashboardComponent {
  users$ = this.service.users$.pipe(
    filter(Boolean)
  );
  activeUsers$ = this.users$.pipe(
    map(users => users.filter(user => user.active)),
  );
  bannedUsers$ = this.users$.pipe(
    map(users => users.filter(user => user.banned)),
  );
  constructor(private service: UsersService) {}
}
````

