# On assertions

Answer the following questions:

1. The following assertion fails `assertTrue(3 * .4 == 1.2)`. Explain why and describe how this type of check should be done.

2. What is the difference between `assertEquals` and `assertSame`? Show scenarios where they produce the same result and scenarios where they do not produce the same result.

3. In classes we saw that `fail` is useful to mark code that should not be executed because an exception was expected before. Find other uses for `fail`. Explain the use case and add an example.

4. In JUnit 4, an exception was expected using the `@Test` annotation, while in JUnit 5 there is a special assertion method `assertThrows`. In your opinion, what are the advantages of this new way of checking expected exceptions?

## Answer

1. The assertion is false because the type (int, float,...) of the result of the calculation is not defined the language will represent it in the most general type which is double, consequently the machine does not know how to represent 0.1 (it represents it approximately) of or this incorrect result. To make this type of comparison, it is necessary to put the result of calculation 3*0.4 in a variable while forcing the type (foat resulta=(float) 3*0.4) then to make the comparison with this variable
	remark="see how the machine codes to make the calculations"

2. AssertEqual compares the value of two objects while assertSame compares the reference of two objects;
assertEqual uses the equals() method which is always redefined to give the order relationship used to compare the values of our objects; assertSame uses == ; however when the equals() method is not redefined then the asserts do the same comparison which is the reference comparison;
Assuming that the equals() method is up to date here are two scenarios:

- scenario of inegality:
	int a=25;
	int b=25;
	assertEquals(a,b)=!assertShame(a,b) 
	assertEquals(a,b)=true because both objects have the same value according to the integer comparison (25=25)
	assertShame(a,b)=false because variables a and b point to different memory locations

- equality scenario:
	int a=25;
	int b=a;
	assertEquals(a,b)=assertShame(a,b) 
	assertEquals(a,b)=true because both objects have the same value according to the integer comparison (25=25)
	assertShame(a,b)=false because variables a and b point to the same memory location

3. You can also use fail() for the following reasons:
- failing a test of a method that is not yet fully implemented:
	@Test
	public void incompleteTest() {
    	fail("Not yet implemented");
	}
- fail a test a precondition that allows the program to continue to run correctly is not checked:
We can call fail() when a result doesn't meet some desired condition:
	@Test
	public void testingCondition() {
	    int result = randomInteger();
	    if(result > Integer.MAX_VALUE) {
		fail("Result cannot exceed integer max value");
	    }
	}
    
it can be used to fail a test when a code that a program does not finish at the right time.
	@Test
	public void returnBefore() {
	    int value = randomInteger();
	    for (int i = 0; i < 5; i++) {
		// returns when (value + i) is an even number
		if ((i + value) % 2 == 0) {
		    return;
		}
	    }
	    fail("Should have returned before");
	}

4. This new way of checking exceptions allows us to test the raising of an exception in the same way as other test cases, and also allows us to do a finer processing of the tests on exception raising:
 We can first check that the execution is raised, which is a test case, and then check properties on the raised exception
