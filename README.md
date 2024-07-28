BEHAVIORAL DESIGN PATTERNS
Command pattern:
interface Command { void execute(); void undo(); }

class TextEditor { StringBuilder text = new StringBuilder();
    void type(String words) { text.append(words); }
    void deleteLast(int length) { text.setLength(text.length() - length); }
    String getText() { return text.toString(); } }

class TypeCommand implements Command { TextEditor editor; String words;
    TypeCommand(TextEditor editor, String words) { this.editor = editor; this.words = words; }
    public void execute() { editor.type(words); }
    public void undo() { editor.deleteLast(words.length()); } }

public class Main { public static void main(String[] args) {
    TextEditor editor = new TextEditor();
    Command typeCommand = new TypeCommand(editor, "Hello");
    typeCommand.execute(); System.out.println("Text: " + editor.getText());
    typeCommand.undo(); System.out.println("Text: " + editor.getText()); }
}
Mediator pattern:
interface ChatMediator { void sendMessage(String message, User user); void addUser(User user); }
class ChatMediatorImpl implements ChatMediator {
    private List<User> users = new ArrayList<>();
    public void addUser(User user) { users.add(user); }
    public void sendMessage(String message, User user) { users.stream().filter(u -> u != user).forEach(u -> u.receive(message)); }
}

abstract class User {
    protected ChatMediator mediator; protected String name;
    public User(ChatMediator mediator, String name) { this.mediator = mediator; this.name = name; }
    abstract void send(String message); abstract void receive(String message);
}

class UserImpl extends User {
    public UserImpl(ChatMediator mediator, String name) { super(mediator, name); }
    public void send(String message) { System.out.println(name + ": Sending=" + message); mediator.sendMessage(message, this); }
    public void receive(String message) { System.out.println(name + ": Received=" + message); }
}

public class Main {
    public static void main(String[] args) {
        ChatMediator mediator = new ChatMediatorImpl();
        User user1 = new UserImpl(mediator, "User1"), user2 = new UserImpl(mediator, "User2"), user3 = new UserImpl(mediator, "User3");
        mediator.addUser(user1); mediator.addUser(user2); mediator.addUser(user3); user1.send("Hi All");
    }
}

