# 3.2 Stacks and Queues #

## Stacking ##

Stack is a last-in-first-out data structure that can be easily implemented using Array. We can simply restrict out interface that wraps an Array to just two operations -- push and pop and some other useful methods to emulate stack like functionality. You can read more about stacks here.



We'll make some assumptions before we start the implementation.

	<!--
	  to emulate (эмулировать; имитировать;)
          assumption (предположение; допущение;)
	-->


array[0] would always be the bottom of our stack. array[n] will be the top of the stack.

size method will keep track of the size of our stack.
	<!--
	  keep track of (следить за;)
	-->

push should always return self. pop should always return the value of the popped element.
	<!--
	  to pop (выталкивать данные из стека;)
	-->

Because arrays in Ruby are dynamic, we'll be starting off with a simple implementation of building a stack that can be grown as we add elements to it.

	<!--
	  start off (начинать; начинаться;)
	-->

	class Stack 
	  def intialize
	    @store = Array.new
	  end
	
	  def pop
	    @store.pop
	  end

	  def push(element)
	    @store.push(element)
	    self
	  end

	  def size
	    @store.size
	  end
	end

The Stack class maintains an unbound @store array and simply calls pop and push on them. size returns the length of the array.

	<!--
	  to maintain (содержать;сохранять;)
	  unbound (несвязанный;)
	-->

Modify the above example to make the Stack class statically sized. push and pop should return nil if the stack is overflowing or underflowing respectively. Implement private predicate methods full? and empty? and public method size that returns the length of the stack, and look that returns the value on the top of the stack.

	<!--
	  statically (статически;)
	  overflow (переполнение;выход за вернюю границу стека;)
	  underflow (выход за нижнюю границу стека;)
	  respectively (соответственно;)
	  predicate method - a method that returns true or false	
	-->

	class Stack
  	  def initialize(size)
	    @size = size
	    @store = Array.new(@size)
	    @top = -1
	  end
  
	  def pop
	    if empty?
	      nil
	    else
	      popped = @store[@top]
	      @store[@top] = nil
	      @top = @top.pred
	      popped
	    end
	  end
  
	  def push(element)
	    if full? or element.nil?
	      nil
	    else
	      @top = @top.succ
	      @store[@top] = element
	      self
	    end
	  end
  
	  def size
	    @size
	  end
  
	  def look
	    @store[@top]
	  end
  
	  private
  
	  def full?
	    @top == (@size - 1)
	  end
  
	  def empty?
	    @top == -1
	  end
	end

## Queueing ##

Queue is a first-in-first-out data structure in which the two major operations are enqueue and dequeue. enqueue adds an element to the rear of the queue and dequeue removes an element from the front of the queue. You can read more about queues here.

	<!--
	  enqueue (добавить в очередь;)
	  dequeue (удалить из очереди;)
	  rear (сзади; задняя сторона;)
	-->


array[0] will be the rear of our queue. array[n] will be the front of the queue.

In this example, we'll use unshift to insert elements and pop to remove an element. This is because our queue is again, unbounded and can be grown as we keep on adding elements. These methods also allow us to not store any front or rear state in the class.

	class Queue
	  def initialize
	    @store = Array.new
	  end

	  def dequeue
	    @store.pop
	  end

	  def enqueue(element)
	    @store.unshift(element)
	    self
	  end

	  def size
	    @store.size
	  end
	end

Neat and simple.



Write your own implementation of a Queue it also statically sized. Like in the stack exercise before, enqueue and dequeue should return nill for overflow or underflow.

	<!--
	  Neat and simple (ясно и просто)
	-->
	class Queue
	  def initialize(size)
	    @size = size
	    @store = Array.new(@size)
	    @head, @tail = -1, 0
	  end
  
	  def dequeue
	    if empty?
	      nil
	    else
	      @tail = @tail.succ
	
	      dequeued = @store[@head]
	      @store.unshift(nil)
	      @store.pop
	      dequeued
	    end
	  end
  
	  def enqueue(element)
	    if full? or element.nil?
	      nil
	    else
	      @tail = @tail.pred
	      @store[@tail] = element
	      self 
	    end
	  end
  
	  def size
	    @size
	  end
  
	  private
  
	  def empty?
	    @head == -1 and @tail == 0
	  end

	  def full?
	    @tail.abs == (@size)
	  end
	end


