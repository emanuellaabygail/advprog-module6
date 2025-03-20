**Emanuella Abygail - 2306152185**

## Milestone 1
The `handle_connection` method reads and prints an incoming HTTP request from a TcpStream. First, it creates a buffered reader `buf_reader` that wraps the `TcpStream` in `BufReader`. The, it read the HTTP request line by line dan print the HTTP request.

## Milestone 2
![Commit 2 screen capture](/assets/image_1.png)
The `handle_connection` processes HTTP requests and sends back an HTML page as a response. It reads the HTTP request line by line and stores it. Then, it prepares the HTTP response by setting the status to `200 OK`, reads the `hello.html` file that will be used as the response body, and gets the file size for the `Content-Length` header. After reading the request and preparing the response, it sends the response to the client.

## Milestone 3
![Commit 3 screen capture](/assets/image_2.png)
In the beginning, the response handling in `handle_connection` was written with if else block containing similar logic for reading files and writing response where the difference lies only in the status line and the html filename. By splitting the respose, we extract the request line from the client request, determine the status line and filename based on the request path, read the file according to the filename we determined before, and format the response and send it back to the client. 

The refactoring was needed because both the `if` and `else` block contained similar logic, making our code redundant. Both the branches read a file, measured its length, formatted a response string, and wrote the response to the stream. This redundancy violates the clean code and, though this is not OOP, the Don't Repeat Yourself principle, possibly making it harder to maintain inthe future. Refactoring simplifies this by separating the decision-making to determine the status and filename from the response construction. Therefore, each of the branches only determine the filename and status while the others are done outside the branch and written only once in our code. By refactoring, we reduce duplicaiton, make future changes easier and our program more maintainable, and improve the readability of our code. So, we keep the logic clean and scalable while ensuring that our web can handle the requests correctly.

## Milestone 4
When we run `/sleep` and then `/`, the browser will take some time to load in both. After the `/sleep` is loaded, the `/` is instantly loaded. Although our program only make it sleep or 5 seconds in `/sleep`, we still have to wait when loading the `/`. This is because `handle_function` function runs in a single-threaded environment and we need to wait for one request to be handled for the next to be handled. So because the `/sleep` request in handled first and it requires our program to delay for 5 seconds, the browser have to wait for 5 seconds, handle the `/sleep` request, then handle the `/` request. If multiple users try to access the server at the same time, they will experience delays because the server handles only one request at a time. This is inefficient and would lead to slow response times under high traffic.

## Milestone 5
Now, the web server can handle multiple requests at the same time using ThreadPool. Instead of creating a new thread for every request, which can be slow, the ThreadPool creates a fixed number of worker threads that are always running and waiting for tasks. Each worker has an id and a thread that loops forever, checking for new tasks. The ThreadPool uses a channel to send tasks to these workers. When execute is called, the task is sent through the channel, and one of the workers picks it up and runs it. This way, requests are handled faster, and the server does not waste time creating and destroying threads repeatedly.

## Bonus
For the bonus, we make a new function `build` to replace teh `new` function. We replace the `new` function with a `build` function in the `ThreadPool` struct. Before, if the size was 0, the program would panic and stop running. Now, `build` returns a Result, so if the size is 0, it gives an error message instead of crashing. This makes the program more reliable because it allows handling errors safely instead of forcing the program to exit. Everything else in the program stays the same, but now we can check if `build` succeeds before using the thread pool. By using `build`, we can handle error better.



<br>
<br>
<br>

**Disclaimer**: I write the reflection after I am finished with each milestone, but I didn't realise I put the `README.md` file in the parent directory of this one and that is why it seems like I commited 4 reflections at once 🙏