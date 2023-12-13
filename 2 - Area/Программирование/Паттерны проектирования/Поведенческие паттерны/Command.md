# Command

Command - паттерн проектирования, который превращает запросы в объекты, позволяя передавать их как аргументы при вызове методов, ставить запросы в очередь, логировать их, а также поддерживать отмену операций.

## Пример

Представим, что у нас есть `UserService`, который сохраняет данные о пользователе. И есть несколько точек входа в этот сервис. В нашем примере это `Controller`, `WebSocket`, `SyncUsers`. Но тут приходит запрос на реализацию логирования, очереди, отложенного запуска и возврата данных.

![Command](command1.png)

Вот тут и приходит на помощь паттерн Команда

![Command](command2.png)

Команда - это отдельный класс, все сервисы не будут обращаться к нашему `UserService`, они будут отправлять команду в класс `addUser`, и в зависимости от команды, класс будет выполнять действия.

```tsx
class User {
	constructor(public userId: number) {}
}

class CommandHistory {
	public commands: Command[] = [];
	push(command: Command) {
		this.commands.push(command);
	}
	remove(command: Command) {
		this.commands = this.commands.filter(c => c.commandId !== command.userId);
	}
}

abstract class Command {
	public commandId: number;

	abstract execute(): void;
	constructor(public history: CommandHistory) {
		this.commandId = Math.random();
	}
}

class AddUserCommand extends Command {
	constructor(
		private user: User, 
		reciever: UserService, 
		history: CommandHistory
		) {
			super(history);
		}
	execute(): void {
		this.receiver.saveUser(this.user);
		this.history.push(this);
	}
	undo() {
		this.receiver.deleteUser(this.user.userId)
		this.history.remove(this);
	}
}

class UserService {
	saveUser(user: User) {
		console.log(`Сохраняю пользователя с id ${user.userId}`);
	}
	deleteUser(userId: number) {
		console.log(`Удаляю пользователя с id ${userId}`);
	}
}

class Controller {
	receiver: UserService;
	history: CommandHistory = new CommandHistory();
	addReceiver(receiver: UserService) {
		this.receiver = receiver;
	}
	run() {
		const addUserCommand = new AddUserCommand(new User(1), this.receiver, this.history);
		addUserCommand.execute()
	}
}

сonst controller = new Controller();
controller.addReceiver(new UserService());
controller.run
```