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

The applicability of Command design pattern can be found in these cases below: