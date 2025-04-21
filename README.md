# Solid-Principle


Solid Principle :
===============

S -> Single Responsiblity Principle :
------------------------------------

Every Java class must perform a single functionality.

Implementation of multiple functionalitues in a single class mashup the code.

		Example :

		Bank Application.....

		1. Deposit/Withdraw -> separate service.
		2. Loan(Vehi/Edu/JwelLoan) -> separate service.
		3. Print( PassBook printing..) -> separate service.
		4. Notification (email / otp) -> separate service.

O -> Open-Closed Principle:
--------------------------

According to new requirements the module should be open for extension but closed for modification.

Note: User should not modify the NotificationService....they will extends the other services.like email, whatsapp and other services. Rather than modifiying the NotifcationService, we can extend the service.

		Example :

		NotificationService.java:
		=========================

		public class NotificationService{
		 public void sendOTP(String medium);
		 public void sendTransactionReport(String medium);
		}

		EmailNotificationService.java :
		=============================

		public class EmailNotificationService implements NotificationService{

		public void sendOTP(String medium){
		// write logic to integrate with email api
		}

		public void sendTransactionReport(String medium){

		}
		}

		MobileNotificationService.java:
		==============================

		public class MobileNotificationService implements NotificationService{

		public void sendOTP(String medium){
		//write the logic to send otp to mobile
		//twillo api
		}

		public void sendTransactionReport(String medium){
		//write the logic to send otp to mobile
		//twillo api
		}
		}

		WhatsAppNotificationService.java:
		================================

		public class WhatsAppNotificationService implements NotificationService{
		public void sendOTP(String medium){
		//write the logic to integrate whatsapp api

		}

		public void sendTransactionReport(String medium){
		//write the logic to send otp to mobile

		}
		}
		
L -> Liskov Substitution Principle(LSP):
---------------------------------------

It applied to inheritance in such a way that the derived classes must be completely substitutable for their base classes. In other words, if class A is subtype of class B, then we should be able to replace B with A without interrupting the behavior of the program.

Violating LSP:
=============

SocialMedia.java:
================

		public abstract class SocialMedia{

		//@support  Whatsapp,Facebook,Instagram
		public abstract void chatWithFriend();

		//@support Facebook,Instagram
		public abstract void publishPost(Object post);

		//@support whatsapp,Facebook,Instagram
		public abstract void sendPhotosAndVideos();

		//@support Whatsapp,Facebook
		public abstract void groupVideoCall(String ... users);

		}

Facebook.java:
-------------

		public class Facebook extends SocialMedia{

		public  void chatWithFriend(){

		}

		public void publishPost(Object post){

		}

		public void sendPhotosAndVideos(){

		}

		public void groupVideoCall(String ... users){

		}

		}

Note: 

Here Facebook is replaced by SocialMedia. derived classes must be completely substitutable for their base classes.



Whatsapp.java:
-------------- 

		public class Whatsapp extends SocialMedia{

		// No override all the methods from SocialMedia.

		public  void chatWithFriend(){

		}

		public void publishPost(Object post){

		// Not applicable this method for whatsapp.

		}

		public void sendPhotosAndVideos(){

		}

		public void groupVideoCall(String ... users){

		}

		}

Note: 

Whatsapp can not be substitute for derived class ie SocialMedia. It is not in Sync and it is not supported Liskov principle.

Instagram.java:
--------------

		public class Instagram extends SocialMedia{

		// No override all the methods from SocialMedia.

		public  void chatWithFriend(){

		}

		public void publishPost(Object post){

		}

		public void sendPhotosAndVideos(){

		}

		public void groupVideoCall(String ... users){

		// Not applicable this method for whatsapp.


		}

		}

Note: 

Instagram can not be substitute for derived class ie SocialMedia. It is not in Sync and it is not supported Liskov principle.

-------------------------------------   Solution here ---------------------------------------------------------------

Step 1: Create solution pkg.

Step 2: Createa interfce ....

		a. public interface SocialMedia{

		   public void chatWithFriend();
		   public void sendPhotosAndVideos();
		   
		   }
		   
		b. public interface PostMediaManager{

		   public void publishPost(Object post);
		   
		   }
		   
		c. public interface SocialVideoCallManager{

		   public void groupVideoCall(String....users);
		   
		   }

Step 3: Implements SocialMedia interface in the corresponding class.

a. Instagram.java:

   public class Instagram implements SocialMedia,PostMediaManager{
   
     	public  void chatWithFriend(){
          // write the logic here.
		}		

		public void sendPhotosAndVideos(){
          // write the logic here.
		}
 
        public void publishPost(Object post);
		   // write the logic here.
		   }
    }

b. whatsapp.java :	
	
    public class Whatsapp implements SocialMedia,SocialVideoCallManager{
   
     	public  void chatWithFriend(){
          // write the logic here.
		}		

		public void sendPhotosAndVideos(){
          // write the logic here.
		}
 
        public void groupVideoCall(String ... users){
		// write the logic here.
		}
    }
 

Interface Segregation Principle (ISP):
=====================================

I -> The principle states that the larger interfaces split into smalller ones.Because the implementation classes use only the methods that are required. We should not force the client to use the methods that they do not want to use.

The goal of the interface segregation principle is similar to the single responsibility principle. Lets understand the principle through an example.

Step 1 : UPIPayments ( Interface )

     public interface UPIPayments{
		 public void payMoney();
		 public void getScratchCard();
		 public void getCashBackAsCreditBalance();
		 }
		 
Step 2 : GooglePay (Class)

     public class GooglePay implements UPIPayments{
		
        public void payMoney(){
		}
		
		public void getScratchCard(){
		}		
		
		public void getCashBackAsCreditBalance(){
		
		}
		}
		
Step 3: PayTm (Class)

     public class Paytm implements UPIPayments{
		
		 public void payMoney(){
		}
		
		public void getScratchCard(){
		}		
		
		public void getCashBackAsCreditBalance(){
		 // Not applicable to PayTm
		}
		}

Step 4: Phonepe (Class)

     public class Phonepe implements UPIPayments{
		
		 public void payMoney(){
		}
		
		public void getScratchCard(){
		}		
		
		public void getCashBackAsCreditBalance(){
		 // Not applicable to Phonepe
		}
		}
   
Note :

     Here, resolve this issue. if it is not applicable those methods create separate interface and implement.And remove this getCashBackAsCreditBalance abstract method from UPIPayments.
	 
Step 5 : Create new interface called CashBackManager.

    public interface CashBackManager{
	  public void getCashBackAsCreditBalance();
	  }
	  
Now implements PayTm and Phonepe,  without getCashBackAsCreditBalance abstract method or remove this method from implementation class. And this getCashBackAsCreditBalance method implement in only GooglePay.

Step 6:

    GooglePay (Class)

    public class GooglePay implements UPIPayments,CashBackManager{
		
        public void payMoney(){
		}
		
		public void getScratchCard(){
		}		
		
		public void getCashBackAsCreditBalance(){
		
		}
		}
		
D -> Dependency Inversion Principle(DIP):
-----------------------------------------

The principle states that we must use abstraction(abstract classes and interfaces) instead of concrete implementations. High level modules should not depend on the low-level module but both should depend on the absctraction.

Step 1: 

		CreditCard.java
		----------------
		
		public class CreditCard{
		
		public void doTranscation(long amount){
		System.out.println("Payment using Creditcard");
		}
		}
		
		
		DebitCard.class
		----------------
		
		public class DebitCard{
		
		public void doTranscation(long amount){
		System.out.println("Payment using DebitCard");
		}
		}
		
Step 2 :

      ShoppingMall.java
	   ------------------
	   
	   public class ShoppingMall{
	   
	   private DebitCard / CreditCard  debitCard / creditCard;
	   
	   public ShoppingMall(DebitCard debitCard){
	   this.debitCard = debitCard;
	   }
	   
	   public void doPurchaseSomething(long amount){
	   debitcard.doTranscation(amount);
	   }
	   
	   public static void main(String args[]){
	   DebitCard debitCard = new DebitCard();
	   // Here is the issue, it is tightly coupled with debitCard or creditCard. if you are not passing //will get error.
	   ShoppingMall shoppingMall = new ShoppingMall(debitCard);  
	   shoppingMall.doPurchaseSomething(5000);
	   }
	   }
	   
	   
Step 3 : Solution for this.

      Create interface common BankCard, it contains CreditCard and DebitCard. Implement this BankCard.
	   
	   public interface BankCard{
	   void doTranscation(long amount);
	   }
	   
	   Now implement this interface with Credit and Debit card.
	   
	    public class CreditCard implements BankCard{
		
		public void doTranscation(long amount){
		System.out.println("Payment using Creditcard");
		}
		}
		
		
		DebitCard.class
		----------------
		
		public class DebitCard implements BankCard{
		
		public void doTranscation(long amount){
		System.out.println("Payment using DebitCard");
		}
		}
	   
	   
Step 4 : Now BankCard pass into ShoppingMall.

     public class ShoppingMall{
	   
	   private BankCard bankCard;
	   
	   public ShoppingMall(BankCard bankCardCard){
	   this.bankCard = bankCard;
	   }
	   
	   public void doPurchaseSomething(long amount){
	   debitcard.doTranscation(amount);
	   }
	   
	   public static void main(String args[]){
	   BankCard bankCard = new DebitCard();
	   ShoppingMall shoppingMall = new ShoppingMall(bankCard);  
	   shoppingMall.doPurchaseSomething(5000);
	   }
	   }
	   
	   
	   
	   
