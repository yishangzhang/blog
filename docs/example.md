# example usage

The HTML specification is maintained by the W3C.

*[HTML]: Hyper Text Markup Language
*[W3C]: World Wide Web Consortium


``` { .java .no-copy }
Optional<String> message = Optional.ofNullable(record.value()); // (1)
if (message.isPresent()) {
    System.out.println(message.get());
}
```

1.  :man_raising_hand: optional判空处理.

``` py linenums="1" hl_lines="2 4"
def bubble_sort(items):
    for i in range(len(items)):
        for j in range(len(items) - 1 - i):
            if items[j] > items[j + 1]:
                items[j], items[j + 1] = items[j + 1], items[j]
```

=== "C"

    ``` c
    #include <stdio.h>

    int main(void) {
    printf("Hello world!\n");
    return 0;
    }
    ```

=== "C++"

    ``` c++
    #include <iostream>

    int main(void) {
    std::cout << "Hello world!" << std::endl;
    return 0;
    }
    ```


| m | a |
|:----:|:-:|
|a|bdjfnb;srodnbodsrnbo;drbndrndnbjno|
|c|d|

删除线{--deleted--}
下划线{++added++}
高亮{==highlighting==}
tian{~~one ~> asingle~~}  
:smile:

<figure markdown>  
    ![Image title](https://dummyimage.com/600x400/){ width="300" }
    <figcaption>Image 说明</figcaption>
</figure>

- [ ] name 
- [x] age
- [ ] tel

We can use Material Icons :material-airplane:.

We can also use Fontawesome Icons :fontawesome-solid-ambulance:.

That's not all, we can also use Octicons :octicons-octoface:.

