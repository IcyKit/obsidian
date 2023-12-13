# Non-Null и Non-Undefined

Для указания того, что сущность точно существует мы используем оператор `Non-Null` и `Non-Undefined` - `!`. Даже если TypeScript будет предупреждать вас об ошибке, то этот оператор отключит это поведение.

Использовать его стоит только тогда, когда вы на все 100% уверены в наличии сущности. Иначе будут ошибки в рантайме.

```ts
interface User {
  login: string;
  password: string;
  age: number;
  addr: string | undefined;
  parents?: {
    mother?: string;
    father?: string;
  };
}

function sendUserData(obj: User, db?: string): void {
  console.log(obj.parents!.father?.toLowerCase(), db!.toLowerCase());
}
```