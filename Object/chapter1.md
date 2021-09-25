# Chapter1 객체, 설계

## 티켓 판매 애플리케이션 구현하기

### Invitation
```java
//이벤트 당첨자에게 발송되는 초대장
public class Invitation {
    private LocalDateTime when;
}
```
### Ticket
```java
//공연을 관람하기 원하는 모든 사람들은 티켓을 소지
public class Ticket {

    private Long fee;

    public Long getFee() {
        return fee;
    }
}
```
### Bag
````java
//관람객이 소지품을 보관할 Bag
public class Bag {

    private Long amount;
    private Invitation invitation;
    private Ticket ticket;

    public Bag(Long amount) {
        this(null, amount);
    }

    public Bag(Invitation invitation, Long amount) {
        this.invitation = invitation;
        this.amount = amount;
    }

    public boolean hasInvitation() {
        return invitation != null;
    }

    public boolean hasTicket() {
        return ticket != null;
    }

    public void setTicket(Ticket ticket) {
        this.ticket = ticket;
    }

    public void minusAmount(Long amount) {
        this.amount -= amount;
    }

    public void plusAmount(Long amount) {
        this.amount += amount;
    }
}
````
### Audience
```java
//관람객
public class Audience {

    private Bag bag;

    public Audience(Bag bag) {
        this.bag = bag;
    }

    public Bag getBag() {
        return bag;
    }
}
```
### TicketOffice
```java
//매표소
public class TicketOffice {

    private Long amount;
    private List<Ticket> tickets = new ArrayList<>();

    public TicketOffice(Long amount, Ticket... tickets) {
        this.amount = amount;
        this.tickets.addAll(Arrays.asList(tickets));
    }

    public Ticket getTicket() {
        return tickets.remove(0);
    }

    public void minusAmount(Long amount) {
        this.amount -= amount;
    }

    public void plusAmount(Long amount) {
        this.amount += amount;
    }
}
```
### TicketSeller
```java
//판매원
public class TicketSeller {

    private TicketOffice ticketOffice;

    public TicketSeller(TicketOffice ticketOffice) {
        this.ticketOffice = ticketOffice;
    }

    public TicketOffice getTicketOffice() {
        return ticketOffice;
    }
}
```
### Theater
```java
//소극장
public class Theater {

    private TicketSeller ticketSeller;

    public Theater(TicketSeller ticketSeller) {
        this.ticketSeller = ticketSeller;
    }

    public void enter(Audience audience) {
        if (audience.getBag().hasInvitation()) {
            Ticket ticket = ticketSeller.getTicketOffice().getTicket();
            audience.getBag().setTicket(ticket);
        } else {
            Ticket ticket = ticketSeller.getTicketOffice().getTicket();
            audience.getBag().minusAmount(ticket.getFee());
            ticketSeller.getTicketOffice().plusAmount(ticket.getFee());
            audience.getBag().setTicket(ticket);
        }
    }
}
```
## 무엇이 문제인가
로버트 마틴은 소프트웨어 모듈이 가져야 하는 세 가지 기능에 관해 설명한다.
+ 실행 중에 제대로 동작하는 것
+ 변경을 위해 존재하는 것
+ 코드를 읽는 사람과 의사소통하는 것

쉽게 말해 모듈은 제대로 실행돼야 하고, 변경이 용이해야 하며, 이해하기 쉬어야한다.<br/>
위에 작성한 프로그램은 제대로 실행돼고 있지만 변경이 용이하지 않고 이해하기가 쉽지가 않다.<br/>
왜 그럴까? <br/>

Theater 클래스의 enter메스드가 수행하는 일은 아래와 같다.
> 소극장은 관람객의 가방을 열어 그 안에 초대장이 들어 있는지 살펴본다.
> 가방 안에 초대장이 들어 있으면 판매원은 매표소에 보관돼 있는 티켓을 관람객의 가방 안으로 옮긴다.
> 가방 안에 초대장이 들어 있지 않다면 관람객의 가방에서 티켓 금액만큼의 현금을 꺼내 매표소에 적립한 후에 매표소에 보관돼 있는 티켓을 관람객의 가방 안으로 옮긴다.

문제는 관람객과 판매원이 소극장의 통제를 받는 수동적인 존재라는 점이다.
일반적으로 관람객이 직접 자신의 가방에서 초대장이나 돈을 꺼내 판매원에게 전달하고 판매원은 매표소에 있는 티켓을 관람객에게 전하고 돈을 받아 매표소에 보관한다.<br/>
하지만 현재의 코드는 우리의 상식과는 너무나도 다르게 동작한다. 코드를 읽는 사람과 제대로 의사소통을 못한다.<br/>
그리고 가장 큰 문제는 변경에 취약한 코드라는 거다. 만약 관람객이 가방을 들고 있지 않거나 신용카드로 결제를 한다면 거의 모든 코드에서 수정이 일어날 것이다. <br/>
이렇게 객체 사이의 **의존성**이 높아 **결합도**가 높은 코드이다.<br/>
따라서 설계의 목표는 애플리케이션의 기능을 구현하는 데 필요한 최소한의 의존성만 유지하고 불필요한 의존성을 제거하여 객체 사이의 결합도를 낮춰 변경이 용이한 설계를 만드는 것이다.


## 설계 개선하기
객체를 인터페이스와 구현으로 나누고 인터페이스만 공개하는 것은 객체 사이의 결합도를 낮추고 변경하기 쉬운 코드를 작성할 수 있다.<br/>
핵심은 객체 내부의 상태를 **캡슐화**하고 객체 간에 오직 메시지를 통해서만 상호작용하도록 만드는 것이다. **결합도**를 낮추고 **응집도**를 높인다.
### Audience
```java
public class Audience {

    private Bag bag;

    public Audience(Bag bag) {
        this.bag = bag;
    }

    public Long buy(Ticket ticket) {
        return bag.hold(ticket);
    }
}
```
### Bag
```java
public class Bag {

    private Long amount;
    private Invitation invitation;
    private Ticket ticket;

    public Long hold(Ticket ticket) {
        if (hasInvitation()) {
            setTicket(ticket);
            return 0L;
        } else {
            setTicket(ticket);
            minusAmount(ticket.getFee());
            return ticket.getFee();
        }
    }

    public Bag(Long amount) {
        this(null, amount);
    }

    public Bag(Invitation invitation, Long amount) {
        this.invitation = invitation;
        this.amount = amount;
    }

    private boolean hasInvitation() {
        return invitation != null;
    }

    private void setTicket(Ticket ticket) {
        this.ticket = ticket;
    }

    private void minusAmount(Long amount) {
        this.amount -= amount;
    }

}
```
### Theater
```java
public class Theater {

    private TicketSeller ticketSeller;

    public Theater(TicketSeller ticketSeller) {
        this.ticketSeller = ticketSeller;
    }

    public void enter(Audience audience) {
        ticketSeller.sellTo(audience);
    }
}
```
### TicketOffice
```java
public class TicketOffice {

    private Long amount;
    private List<Ticket> tickets = new ArrayList<>();

    public TicketOffice(Long amount, Ticket... tickets) {
        this.amount = amount;
        this.tickets.addAll(Arrays.asList(tickets));
    }

    public void sellTicketTo(Audience audience) {
        plusAmount(audience.buy(getTicket()));
    }

    private Ticket getTicket() {
        return tickets.remove(0);
    }

    private void plusAmount(Long amount) {
        this.amount += amount;
    }
}
```
### TicketSeller
```java
public class TicketSeller {

    private TicketOffice ticketOffice;

    public TicketSeller(TicketOffice ticketOffice) {
        this.ticketOffice = ticketOffice;
    }

    public void sellTo(Audience audience) {
        ticketOffice.sellTicketTo(audience);
    }
}
```
이처럼 Theater가 몰라도 되는 세부사항을 Audience와 TicketSeller 내부로 감춰 **캡슐화**하는 것이다.<br/>
불필요한 세부사항을 **캡슐화**하는 것은 객체의 **자율성**을 높이고 낲은 **결합도**와 높은 **응집도**를 가지고 협력하도록<br/>
최소한의 의존성만을 남기는 것이 훌륭한 객체지향 설계다.

## 정리
훌륭한 객체지향 설계란 협력하는 객체 사이의 의존성을 적절하게 관리하는 설계다.

