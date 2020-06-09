# activeMQ
___activeMQ는 JMS로 구현한 메시지 지향 미들웨어(MOM)이다.___

### 메시지 지향 미들웨어(Message Oriented Middleware: MOM)
+ 분산 시스템 간 메시지를 주고 받는 기능을 지원하는 소프트웨어나 하드웨어 인프라

### 메시지 큐 (Message Queue: MQ)
+ MOM을 구현한 시스템

### 브로커(Broker)
+ Message Queue 시스템

### AMQP(Advanced Message Queueing Protocol)
+ 메시지 지향 미들웨어를 위한 프로토콜

### JMS(Java Message Service)
+ 자바 메시지 서비스(Java Message Service: JMS)는 자바 프로그램이 네트워크를 통해 데이터를 송수신하는 자바 API이다.


## activeMQ
+ Apache ActiveMQ는 가장 대중적이고 강력한 오픈 소스 메세징 그리고 통합 패턴 서버이다.
+ Apache ActiveMQ는 빠르며, 다양한 언어간의 클라이언트 및 프로토콜을 지원하고,</br>
  사용하기 쉬운 엔터프라이즈 통합 패턴 및 많은 고급 기능을 제공하면서 JMS 1.1 및 J2EE 1.4를 완벽하게 지원한다.
+ MOM(메시지 지향 미들웨어)이다.
+ ActiveMQ는 JMS를 지원하는 클라이언트를 포함하는 브로커, 자바뿐만 아니라 다양한 언어를 이용하는 시스템간의 통신을 할 수 있게 해준다.
+ 간단히 정의하면 클라이언트 간 메시지를 송수신 할 수 있는 오픈 소스 Broker(JMS 서버)이다.</br>
___=>즉, activeMQ에서 JMS는 핵심요소이다.___

## JMS
+ JMS는 자바 기반의 MOM(메시지 지향 미들웨어)API 이며 둘 이상의 클라이언트 간의 메시지를 보냅니다.
+ JMS는 자바 플랫폼, 엔터프라이즈 에디션(EE) 기반이며, 메시지 생성, 송수신, 읽기를 한다.</br>
  또한 비동기적이며 신뢰할만하고 느슨하게 연결된 서로 다른 분산 애플리케이션 컴포넌트 간의 통신을 허용한다.
+ JMS의 핵심 개념은 Message Broker와 Destination 입니다.

Message Broker: 목적지에 안전하게 메시지를 건네주는 중개자 역할</br>
Destination: 목적지에 배달된 2가지 메시지 모델 QUEUE, TOPIC</br>

Queue: Point to Point( Consumer는 메시지를 받기 위해 경쟁한다.)
Topic: Publish to Subscribe


## activeMQ 메시지 처리 구조
Producuer(생산자)가 Message를 Queue/Topic에 넣어두면, Consumer가 Message를 가져와 처리하는 방식</br>
___=> 기본적으로 Message를 생산하는 Producer, activeMQ Broker(Server), Message를 소비하는 Consumer로 구성되어 있다.___
</br>
</br>
</br>

+ QUEUE 모델의 경우 메시지를 받는 Consumer가 다수일 때 연결된 순서로 메시지는 제공된다.
+ TOPIC 모델의 경우 메시지를 받는 Consumer가 다수일 때 메시지는 모두에게 제공된다.
+ Consumer 에서는 메시지를 받을 때까지 block 상태인 Receive() 메소드와 리스너를 통해 nonblock 상태로 메시지를 가져올 수 있다.


## activeMQ의 장점
1. 분리: 대기열은 시스템 사이에 있으며, 하나의 시스템 장애는 다른 대기열에 영향을 주지 않는다.</br>
   메시지 통신은 대기열을 통해 이루어진다. 시스템이 가동 중일 때도 계속 작동한다.</br>
   ___(클라이언트와 서버간의 연결과 큐 대기열의 역할이 분리)___
  
2. 복구 지원: 큐의 처리가 실패하면 나중에 메시지를 복원 할 수 있습니다.
3. 신뢰성: 클라이언트 요청을 처리하는 시스템을 생각해보자.</br>
   정상적인 경우 시스템은 분당 100건의 요청을 받는다. 이 시스템은 요청 수가 평균을 넘어서는 경우 신뢰할 수 없다.</br>
   이 경우 Queue는 요청을 관리 할 수 있으며 시스템 처리량을 기초로 주기적으로 메시지를 전달할 수 있다.
   ___(큐에서 들어온 메시지에 대한 처리를 관리하기 때문에 신뢰할 수 있는 시스템)___
4. 비동기 처리: 클라이언트와 서버 통신이 비 차단이다. 클라이언트가 서버에 요청을 보내면 응답을 기다리지 않고 다른 작업을 수행 할 수 있다.</br>
   응답을 받으면 클라이언트는 언제든지 처리 할 수 있다.
   
   
## Broker

activeMQ에는 language, clustering, jmx monitoring, message, embedded broker, Persistence, DB등 여러 특징이 있다.<br/>

1. EMbeded Broker
Java에서 activeMQ Broker Server를 직접 구현하겠다면 다음과 가이 3가지 방법으로 구현할 수 있다.
> Maven(Spring), xBean, Broker 객체를 직접 생성

(1)
```java
public class MQServer {

	public static void main(String[] args) {

		try {
                      // ActiveMQConnectionFactory를 사용하고 VM 커넥터를 URI로 사용하여 내장 브로커를 만들 수 있다.
		                //BrokerService broker= ActiveMQConnectionFactory("vm://localhost?broker.persistent=false");
                 //BrokerService broker = BrokerFactory.createBroker("broker:()/master");

                BrokerService broker = new BrokerService();

				broker.setBrokerName("Broker1");
				broker.setUseJmx(true); // check true or false
				broker.setPersistent(true);
				// broker.addConnector("tcp://localhost:61616");

				/// start() 메소드가 저장소 잠금 보류를 차단할 때 유용합니다 (예 : 슬레이브 시작).
				TransportConnector conn = new TransportConnector();
				conn.setUri(new URI("tcp://localhost:61616"));

				// failover:// 브로커 클러스터에 대한 클라이언트 연결을 업데이트
				// conn.setUpdateClusterClients(true);
				broker.addConnector(conn);
				broker.start();

		} catch (Exception e1) {
			// TODO Auto-generated catch block
			e1.printStackTrace();
		}
    }
}

```

