<p>Pattern matching in Scala is a powerful feature that allows you to check
a value against a pattern. It can match complex expressions, decompose
values, and is deeply integrated with Scala&#39;s case classes.</p>
<p>## Real-World Use Cases</p>
<p>### 1. Handling Multiple Data Types</p>
<p>Function may return different types of results. Pattern matching allows
handling each type appropriately.</p>
<p>```scala code sealed trait ApiResponse case class
SuccessResponse(data: String) extends ApiResponse case class
ErrorResponse(message: String) extends ApiResponse</p>
<p>def handleResponse(response: ApiResponse) = response match { case
SuccessResponse(data) =&gt; println(s&quot;Received data: $data&quot;) case
ErrorResponse(message) =&gt; println(s&quot;Error: $message&quot;) }</p>
<p>### 2. Extracting Values from Case Classes Case classes are used to
model immutable data. Pattern matching is used for deconstructing these
classes.</p>
<p>```scala code case class User(id: Int, name: String)</p>
<p>val user = User(1, &quot;Alice&quot;)</p>
<p>user match { case User(id, name) =&gt; println(s&quot;User ID: $id, Name:
$name&quot;) } 3. Recursive Algorithms Pattern matching is often used in
recursive algorithms, especially with recursive data structures like
trees or lists.</p>
<p>scala code sealed trait LinkedList[+A] case object End extends
LinkedList[Nothing] case class Pair[+A](head: A, tail:
LinkedList[A]) extends LinkedList[A]</p>
<p>def sum(list: LinkedList[Int]): Int = list match { case End =&gt; 0 case
Pair(head, tail) =&gt; head + sum(tail) } 4. Handling Optional Values
Scala&#39;s Option type is commonly handled using pattern matching.</p>
<p>scala code val name: Option[String] = Some(&quot;Bob&quot;)</p>
<p>name match { case Some(n) =&gt; println(s&quot;Name is $n&quot;) case None =&gt;
println(&quot;Name is not provided&quot;) } 5. Actor Messages in Akka Actors in
Akka use pattern matching to decide on actions based on received
messages.</p>
<p>scala code import akka.actor.Actor</p>
<p>class MyActor extends Actor { def receive = { case &quot;test&quot; =&gt;
println(&quot;Received test&quot;) case _ =&gt; println(&quot;Received unknown
message&quot;) } } 6. JSON Parsing Pattern matching is used for extracting
data from various JSON structures safely.</p>
<p>scala code import play.api.libs.json._</p>
<p>val json: JsValue = Json.parse(&quot;&quot;&quot;{&quot;name&quot;:&quot;John&quot;,
&quot;age&quot;:30}&quot;&quot;&quot;)</p>
<p>json match { case JsObject(fields) =&gt; println(&quot;Processing an object&quot;)
case JsString(value) =&gt; println(s&quot;Found string: $value&quot;) // more
cases } 7. Simplifying Complex Conditionals Pattern matching can
simplify complex conditional logic.</p>
<p>scala code</p>
<p>def gameResult(score: (Int, Int)) = score match { case (us, them) if us
&gt; them =&gt; &quot;We won!&quot; case (us, them) if us == them =&gt; &quot;It&#39;s a
draw.&quot; case _ =&gt; &quot;We lost.&quot; } Pattern matching in Scala allows for
clean, expressive, and concise code that can handle complex logic in an
easily readable form.</p>
