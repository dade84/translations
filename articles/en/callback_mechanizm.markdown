# Callback mechanism #

  A main thread to manage the other threads, without polling method to check the state of each thread, but certain state in the sub-thread notify the main thread, ah, someone to press me, ah, I value reaches 100 (termed trigger some kind of event) so that the main thread recieves these messages and then call the appropriate method based on the message type.
  An example, I (the main thread) traveled to Beijing, I want to call when the car to "get off", if not back to the calling mechanizm, I want to continue to ask the driver to not ah? If I ask more than three times I call that driver effort than I large, he must fight me, if you go back to the calling mechanism, that is, with a sub-thread (allows driver to assume this role) in that run, I wen to when to notify "get off()", instead of after a while he asked again, so that I can save time sleeping or chat with car crush.

	<!-- 
	  thread (поток;)
	  polling (опрос; голосование;)
	  certain (конкретный;определенный; известный; точный;)
	  to notify (извещать; уведомлять;)
	  to press (давить; оказывать давление;)
	  to term (выражать; называть; именовать;)
	  to trigger (инициировать; вызывать; приводить в действие; начинать;) 
	  
         
	  effort ()
	  in that (по той причине, что; в том плане, что; в том, что;)
          after a while (через некоторое время; спустя некоторое время;)


	-->



callback mechanizm for the C language, usually with a callback mechanism to achieve multiplexing, which is a commonly used reuse mechanism.
Declare a set of function pointers interface for each supported format, respectively, to achieve, such interface instantiated using these functions, that is, the dispatch table. 
Then, somewhere in the control flow, a dispatch table assignment to the appropriate table variable, the function call is the appropriate format function call.
For example, writing a character device driver is such a mechanism, the programmer function to achieve the open, read, write, close, etc., and then registered in the system, the system will automatically call the user-defined function when the access device.





