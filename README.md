# lab5
## Snippet 1 should produce: <br>
[url.com, `google.com, google.com, ucsd.edu]
## Snippet 2 should produce: <br>
[a.com, a.com(()), example.com]
## Snippet 3 should produce<br>
[https://ucsd-cse15l-w22.github.io]
## code
```
    @Test
    public void testFile9() throws IOException{
        Path fileName = Path.of("test-file9.md");
	    String contents = Files.readString(fileName);
        ArrayList<String> links = MarkdownParse.getLinks(contents);
        ArrayList<String> expected = new ArrayList<String>();
        expected.add("url.com");
        expected.add("`google.com");
        expected.add("google.com");
        expected.add("ucsd.edu");
        assertEquals(expected,links);
    }

    @Test
    public void testFile10() throws IOException{
        Path fileName = Path.of("test-file10.md");
	    String contents = Files.readString(fileName);
        ArrayList<String> links = MarkdownParse.getLinks(contents);
        ArrayList<String> expected = new ArrayList<String>();
        expected.add("a.com");
        expected.add("a.com(())");
        expected.add("example.com");
        assertEquals(expected,links);
    }    

    @Test
    public void testFile11() throws IOException{
        Path fileName = Path.of("test-file11.md");
	    String contents = Files.readString(fileName);
        ArrayList<String> links = MarkdownParse.getLinks(contents);
        ArrayList<String> expected = new ArrayList<String>();
        expected.add("https://ucsd-cse15l-w22.github.io");
        assertEquals(expected,links);
    }    
```
## For my implementation
Snippet 1 passed<br>
Snippet 2, 3 failed<br>
2:
```
1) testFile10(MarkdownParseTest)
java.lang.AssertionError: expected:<[a.com, a.com(()), example.com]> but was:<[a.com, a.com((, example.com]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testFile10(MarkdownParseTest.java:94)
```
3: 
```
2) testFile11(MarkdownParseTest)
java.lang.AssertionError: expected:<[https://ucsd-cse15l-w22.github.io]> but was:<[
    https://www.twitter.com
,
    https://ucsd-cse15l-w22.github.io/
, github.com

And there's still some more text after that.

[this link doesn't have a closing parenthesis for a while](https://cse.ucsd.edu/



]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testFile11(MarkdownParseTest.java:104)
```
## for the reviewed implementation
all 3 snippets failed<br>
1:
```
1) testFile9(MarkdownParseTest)
java.lang.AssertionError: expected:<[url.com, `google.com, google.com, ucsd.edu]> but was:<[url.com, `google.com, google.com]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testFile9(MarkdownParseTest.java:99)
```
2:
```
2) testFile10(MarkdownParseTest)
java.lang.AssertionError: expected:<[a.com, a.com(()), example.com]> but was:<[a.com((]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testFile10(MarkdownParseTest.java:111)
```
3:
```
3) testFile11(MarkdownParseTest)
java.lang.AssertionError: expected:<[https://ucsd-cse15l-w22.github.io]> but was:<[
    https://ucsd-cse15l-w22.github.io/
, github.com

And there's still some more text after that.

[this link doesn't have a closing parenthesis for a while](https://cse.ucsd.edu/



]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testFile11(MarkdownParseTest.java:121)
```
## Answer the following questions
1. my snippet 1 worked.
2. yes. i should probably inspect the rearmost close parenthesis, instead of the next parenthesis. this way i can go pass the potential parentheses inside a link.
3. no, i can't think of a way to change my code within 10 lines to solve this problem, because the position of different parentheses includes line change, and it cannot be solved unless we discard the use of the `readString` method at `MarkdownParse.main`.