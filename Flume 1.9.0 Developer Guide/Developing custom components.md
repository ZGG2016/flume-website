# Developing custom components

[TOC]

## 1、Client

### 1.1、Client SDK

### 1.2、RPC client interface

### 1.3、RPC clients - Avro and Thrift

### 1.4、Secure RPC client - Thrift

### 1.5、Failover Client

### 1.6、LoadBalancing RPC client

## 2、Embedded agent

## 3、Transaction interface

## 4、Sink

## 5、Source

> The purpose of a Source is to receive data from an external client and store it into the configured Channels. A Source can get an instance of its own ChannelProcessor to process an Event, commited within a Channel local transaction, in serial. In the case of an exception, required Channels will propagate the exception, all Channels will rollback their transaction, but events processed previously on other Channels will remain committed.

> Similar to the SinkRunner.PollingRunner Runnable, there’s a PollingRunner Runnable that executes on a thread created when the Flume framework calls PollableSourceRunner.start(). Each configured PollableSource is associated with its own thread that runs a PollingRunner. This thread manages the PollableSource’s lifecycle, such as starting and stopping. A PollableSource implementation must implement the start() and stop() methods that are declared in the LifecycleAware interface. The runner of a PollableSource invokes that Source‘s process() method. The process() method should check for new data and store it into the Channel as Flume Events.

> Note that there are actually two types of Sources. The PollableSource was already mentioned. The other is the EventDrivenSource. The EventDrivenSource, unlike the PollableSource, must have its own callback mechanism that captures the new data and stores it into the Channel. The EventDrivenSources are not each driven by their own thread like the PollableSources are. Below is an example of a custom PollableSource:

```java
public class MySource extends AbstractSource implements Configurable, PollableSource {
  private String myProp;

  @Override
  public void configure(Context context) {
    String myProp = context.getString("myProp", "defaultValue");

    // Process the myProp value (e.g. validation, convert to another type, ...)

    // Store myProp for later retrieval by process() method
    this.myProp = myProp;
  }

  @Override
  public void start() {
    // Initialize the connection to the external client
  }

  @Override
  public void stop () {
    // Disconnect from external client and do any additional cleanup
    // (e.g. releasing resources or nulling-out field values) ..
  }

  @Override
  public Status process() throws EventDeliveryException {
    Status status = null;

    try {
      // This try clause includes whatever Channel/Event operations you want to do

      // Receive new data
      Event e = getSomeData();

      // Store the Event into this Source's associated Channel(s)
      getChannelProcessor().processEvent(e);

      status = Status.READY;
    } catch (Throwable t) {
      // Log exception, handle individual exceptions as needed

      status = Status.BACKOFF;

      // re-throw all Errors
      if (t instanceof Error) {
        throw (Error)t;
      }
    } finally {
      txn.close();
    }
    return status;
  }
}
```

## 6、Channel