---
title: Command Pattern
layout: posts
tags: designPattern
---

"An object that contains a symbol, name or key that represents a list of commands, actions or keystrokes". This is the definition of macto. one that should be familiar to any computer user. From this idea the Command design pattern was given birth.

The Macro represents a command that is built from the reunion of a set of other commands, in agiven order. Just as a macro, the command design pattern encapsulates commands(method calls) in objects allowing us to issue request without knowing the requested operation or the requesting object. Command design pattern provides the operations to queue commands,undo/redo actions and other manipulations.

## Intent

* encapsulate a request in an object.

* allow the parameterization of clients with different requests.

* allows saving the requests in a queue.

## Implementation

The idea and implementation of the Command design pattern is quite simple, as we will see in the diagram below, needing only few extra classes implemented.

![command_implementation](/pictures/command_implementation_-_uml_class_diagram.gif)

The classes participating in the pattern:

**Command** - declares an interface for executing an operation.

**ConcreteCommand** - extends the Command interface, implementing the Execute method by invoking the corresponding operations on Receiver. It defines a link between the Receiver and the action.

**Client** - create a ConcreteCommadn object and set its receiver.

**Invoker** - asks the command to carry out the request.

**Receiver** - knows how to perform the operations.

The Client asks for a command to be executed. The Invoker takes the command, encapsulates it and places it in a queue, in case there is something else to do first, and the ConcreteCommand that is in charge of the requested command, sending its result to the Receiver.

Here is a simple code of a classic implementation of this pattern for placing orders for buying and selling stocks:

![command_example_stocks](/pictures/command_example_stocks_-_uml_class_diagram_s.gif)

The client creates some orders for buying and selling stocks(ConcreteCommands). Then the orders are sent to the agent(Invoker). The agent takes the orders and place them to the StockTrade sysetm(Receiver). The agent keeps an internal queue with the order to be placed. Let's assume that the StockTrade system is closed each Monday, but the agent accepts orders and queue them to be processed later on.

```java
public interface Order {
    public abstract void execute ();
}

// Receiver class.
class StockTrade {
    public void buy() {
        System.out.println("You want to buy stocks");
    }
    public void sell() {
        System.out.println("You want to sell stocks ");
    }
}

// Invoker.
class Agent {
    private Deque ordersQueue = new ArraryDeque();

    public Agent() {
    }
    void placeOrder(Order order) {
        ordersQueue.addLast(order);
        order.execute(ordersQueue.getFirstAndRemove());
    }
}

//ConcreteCommand Class.
class BuyStockOrder implements Order {
    private StockTrade stock;
    public BuyStockOrder ( StockTrade st) {
        stock = st;
    }
    public void execute( ) {
        stock . buy( );
    }
}

//ConcreteCommand Class.
class SellStockOrder implements Order {
    private StockTrade stock;
    public SellStockOrder ( StockTrade st) {
        stock = st;
    }
    public void execute() {
        stock.sell();
    }
}

// Client
public class Client {
    public static void main(String[] args) {
        StockTrade stock = new StockTrade();
        BuyStockOrder bsc = new BuyStockOrder (stock);
        SellStockOrder ssc = new SellStockOrder (stock);
        Agent agent = new Agent();

        agent.placeOrder(bsc); // Buy Shares
        agent.placeOrder(ssc); // Sell Shares
    }
}
```

## Applicability & Examples

The applicability of Command design pattern can be found in these cases below

* parameterizes objects depending on the action they must perform.

* specifies or adds in a queue and execute requests at different moments in time.

* offers support for undoable actions (the execute method can memorize the state and allow going back to that state).

* structures the system in high level operations that based on primitive operations.

* decouple the object that invokes the action from the object that perform the action. Due to this usage it is also known as Producer Consumer design pattern.

For exampel we build a AudioPlayer System for little girl Julia.

Julia has a cassette player which has three buttons Play, Rewind, Stop.
In this example the keybord of the player is the Invoker, Julia is the Client,the player is Receiver.

There are three commands in this example PlayCommand,RewindCommand,StopCommand. Juila don't need to know how these actions executed. She just press down the button then keybord recognize which button is preesed and ask the player to execute that command.

cassette player is a typically command pattern. it decouple Julia and the cassette player.

![cassette player](/pictures/cassette_player.webp)

### Example Code

```java
// Receiver AudioPlayer
public class AudioPlayer {
    public void play() {
        System.out.println("play...");
    }
    public void rewind() {
        System.out.println("rewind...");
    }
    public void stop() {
        System.out.println("stop...");
    }
}

// Command
public interface Command {
    /**
     * execute method
     */
    void execute();
}

// Concrete Command
public class PlayCommand implements Command {
    private AudioPlayer audio;
    public PlayCommand(AudioPlayer audio) {
        this.audio = audio;
    }
    @Override
    public void execute() {
        audio.play();
    }
}

public class RewindCommand implements Command {
    private AudioPlayer audio;
    public RewindCommand(AudioPlayer audio) {
        this.audio = audio;
    }
    @Override
    public void execute() {
        audio.rewind();
    }
}

public class StopCommand implements Command {
    private AudioPlayer audio;
    public StopCommand(AudioPlayer audio) {
        this.audio = audio;
    }
    @Override
    public void execute() {
        audio.stop();
    }
}

// Invoker Keyboard
public class Keypad {
    private Command playCommand;
    private Command rewindCommand;
    private Command stopCommand;
    public void setPlayCommand(Command playCommand) {
        this.playCommand = playCommand;
    }
    public void setRewindCommand(Command rewindCommand) {
        this.rewindCommand = rewindCommand;
    }
    public void setStopCommand(Command stopCommand) {
        this.stopCommand = stopCommand;
    }
    public void play() {
        playCommand.execute();
    }
    public void rewind() {
        rewindCommand.execute();
    }
    public void stop() {
        stopCommand.execute();
    }
}

// Client Julia

public class Julia {
    public static void main(String[] args) {
        //create the receiver
        AudioPlayer audioPlayer = new AudioPlayer();
        //commands
        Command playCommand = new PlayCommand(audioPlayer);
        Command rewindCommand = new RewindCommand(audioPlayer);
        Command stopCommand = new StopCommand(audioPlayer);
        //invoker
        Keypad keypad = new Keypad();
        keypad.setPlayCommand(playCommand);
        keypad.setRewindCommand(rewindCommand);
        keypad.setStopCommand(stopCommand);
        //test
        keypad.play();
        keypad.rewind();
        keypad.stop();
        keypad.play();
        keypad.stop();
    }
}
```

### Macro Command

Suppose the audioPlayer has a record feature, it can remember commands have been executed then can execute these commands when the macro button is pressed.

![macro](/pictures/macro_command.webp)

### Code

```java
public interface MacroCommand extends Command {
    public void add(Command command);
    public void remove(Command command);
}

public class MacroAudioCommand implements MacroCommand {
    private List<Command> commandList = new ArrayList<>();

    @Override
    public void execute() {
        System.out.println("---macro command start---");
        for (Command command : commandList) {
            command.execute();
        }
        System.out.println("---done---");
    }
    @Override
    public void add(Command command) {
        commandList.add(command);
    }
    @Override
    public void remove(Command command) {
        commandList.remove(command);
    }
}

public class Julia {
    public static void main(String[] args) {
        AudioPlayer audioPlayer = new AudioPlayer();
        Command playCommand = new PlayCommand(audioPlayer);
        Command rewindCommand = new RewindCommand(audioPlayer);
        Command stopCommand = new StopCommand(audioPlayer);
        MacroCommand macroCommand = new MacroAudioCommand();
        macroCommand.add(playCommand);
        macroCommand.add(rewindCommand);
        macroCommand.add(stopCommand);
        macroCommand.add(playCommand);
        macroCommand.add(stopCommand);
        macroCommand.execute();
    }
}
```

## Sepcific problems and implementation

Now that we have understood how the pattern works, it's time to take a look at its advantages and flaws, too.

### The intelligence of a command

There are two extremes that a programer must avoid when using this pattern:

1. The command is just a link between the receiver and the actions that carry out the request.

2. The command implementations everything itself, without sending anything to the receiver.

We must always keep in mind the fact that the receiver if one who knows how to perform the operations needed, the purpose of the command being to help the client to delegate its request quickly and to make sure the command ends up where it should.

### Undo and redo actions

As mentioned above, some implementations of the command design pattern include parts for supporting undo and redo actions. In order to do that a mechanism to obtain past states of the Receiver object is needed. In order to achieve this there are two options:

1. Before running each command a snapshot of the receiver stat is stored in memory/ This does not require much programing effort but can not be always applied. For example doing this in a image processing application whould require storing images in memory after aech step, which is partically impossible.

2. Instead of storing receiver object stats, the set of performed operations are stored in memory. In this case the command and receiver classes should implement the liverse algorithms to undo each action. This will require additional programing effort, but less memory will be required. Sometimes for undo/redo actions the command should store more information about the state of the receiver objects. A good idea in such cases is to use the Memento Pattern.

### Asynchronous Method Invocation

Another usage for the command design pattern is to run commands asynchronous in background of an application. In this case the invoker is running in the main thread and sends the requests to the receiver which is running in a separete therad. The invoker will keep a queue of commands to be run and will send them to the receiver while it finishes running them.

Instead of using one thread in which the receiver is running more threads can be created for this. But for performance issues(thread creation is consuming) the number of threads should be limited. In this case the invoker will use a pool of receiver threads to run command asynchronously.

### Adding new commands

The command object decouples the object that invokes the action from the object that performs the action.
There are implementations of the pattern in which the invoker instantitates the concrete command objects. In this case if we need to add a new command type we need to change the invoker as well. And this would violate the Open Close Principle. In order to have the ability to add new commands with minimum of effort we have to make sure that the invoker is aware only about the abstract command class or interface.

### Using composite commands

When adding new commands to the application we can use the composite pattern to grous existing commands in another new command. This way, macros can be created from exisiting commands.

## Hot spot

The main advantage of the command design pattern is that it decouples the object that invokes the operation from the one that know how to perform it. And this advantage must be kept. There are implementations of this design pattern in which the invoker is aware of the concrete commands classes. This is wrong making the implementation more tightly coupled. The invoker should be aware only about the abstract command class.