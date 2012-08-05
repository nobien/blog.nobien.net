---
comments: true
date: 2008-09-12 06:33:23
layout: post
slug: flash-remoting-data-managment
title: Flash Remoting Data Management
wordpress_id: 79
categories:
- actionscript-3
---

I'm currently involved in a project at [Almighty](http://www.almightyboston.com) that involves working with more than ... well .. just me. We've recently come up to the question of data management through Flash Remoting. We're using [Danny Patterson's AS3 Lightweight Remoting Framework](http://osflash.org/as3lrf) (because it's pretty awesome), which solves the question of how to interact with AMFPHP, but now the question turns to "What do you do with the data once it is returned?"  

Here are two solutions that I've been using. However, I'm curious to know what _you_ do for data management, and what are you using to send and receive data through AMFPHP. 


In both solutions, I am using an MVC approach. The DataModel is a Singleton. 

**Solution 1. [ YOUR CLASS ]  -> NEW REMOTING SERVICE INSTANCE ->  ADD LISTENERS FOR RESULTS/FAULTS ->  CALL SERVICE METHOD  ->  MANAGE RETURN IN YOUR CLASS (meaning, update the Data Model)**

For instance: 

`private function init():void
{
    var myRemotingService:RemotingService = new RemotingService();
    myRemotingService.addEventListener( RemotingService.DATA_RESULT , onDataResult );
    myRemotingService.getData();
}

private function onDataResult( event:ResultEvent ):void
{
    DataModel.getInstance().data = event.result;
    // continue to use the data //
}`

This is what I've been typically doing, but I usually have sole ownership over a project.  For me, it's easier to wrap my brain around what is being sent to and received from the database through one class, rather than managing everything in the Remoting Service class.  Everything is packaged in one easy call/return and you would parse the data as you see fit. The downside to this is that if the model is updated, only the class that instantiated the service call would know it. You would only be relying on the DataModel as a class that stores the data, versus managing the data. 

**Solution 2. [ YOUR CLASS ] ->  NEW REMOTING SERVICE INSTANCE  ||  LISTEN FOR THE DATA MODEL CHANGE EVENT  ->  CALL SERVICE METHOD  ->  MANAGE **

`private function Constructor():void
{
    DataModel.getInstance().addEventListener( DataModel.DATA_UPDATE , onDataUpdate );
    callDataUpdate();
}

private function callDataUpdate():void
{
    var myRemotingService:RemotingService = new RemotingService();
    myRemotingService.getData();
}

private function onDataUpdate( event:Event ):void
{
    // parse data from data model here //
}`

In this example, the Remoting Service does all of the listening, and it would update the DataModel. A definite benefit of this structure would be if more than one class is relying on the same data, all instantiated classes will be updated. However, the cost that comes to mind is that the Service class itself is bound to get convoluted with logic, since I'm a pretty meticulous coder.  This could be easily remedied through using a utility class to parse the data as much as possible.  

ie, in the Remoting Service:

`private function onDataCallResult( event:Result ):void
{
    DataModel.getInstance().data = DataParser.parseDataCallResult( event.result );   // This would return an Array OR Variable Object containing your parsed data.
    dispatchEvent( new ResultEvent( RemotingService.DATA_RESULT , false, false, event.result )); 
}`

Again, both solutions are two ways in which I have worked with Remoting, but I'm certain there are other ways (possibly more efficient?) of solving the same problem.
