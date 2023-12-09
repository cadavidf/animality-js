
# 🐍 Python Usage Instructions 📘

## Installation 📦
```bash
pip install animality-py
```

## Example 🌟
```python
import animality
from asyncio import get_event_loop

async def run():
    animal = await animality.get("dog")
    print(animal.name, animal.image, animal.fact)
    random = await animality.random()
    print(random.name, random.image, random.fact)

get_event_loop().run_until_complete(run())
```
This outputs the following text in the terminal:
```
<Animal name="dog" image="..." fact="...">
<Animal name="..." image="..." fact="...">
```

# C/C++ Instructions 🖥️

## Required dependencies:
- libcurl (Linux only)
- pthreads (Linux only, might be already installed by default)
- wininet (Windows only, might be already installed by default)

## Installation 🚀
### Windows (MinGW) or Linux
```bash
git clone https://github.com/animality-xyz/animality.h.git && cd animality.h/
gcc -c animality.c -o animality.o
ar rcs -o libanimal.a animality.o
```

### Windows (Visual C++)
```bash
git clone https://github.com/animality-xyz/animality.h.git && cd animality.h
cl /LD animality.c
```

## Example 📚
The response from requesting to the library is this struct.
```c
typedef struct {
    an_type_t type;    // animal name, as enum
    char * name;       // animal name, as string
    char * image_url;  // animal image URL
    char * fact;       // animal fact
} animal_t;
```

Here is a simple request example to the API:
```c
#include "animality.h"

int main() {
    // create our animal struct
    animal_t animal = AN_EMPTY_ANIMAL;

    // request for an animal
    an_get(AN_DOG, &animal);

    // do stuff with struct
    printf("animal image url: %s
", animal.image_url);
    printf("animal fact: %s
", animal.fact);
    
    // free animal struct once used
    an_cleanup(&animal);

    // global cleanup
    an_cleanup(NULL);

    return 0;
}
```

For an asynchronous request:
```c
#include "animality.h"

// our request callback function!
void callback(const animal_t * animal) {
    printf("animal image url: %s
", animal->image_url);
}

int main() {
    // create separate thread for requesting to the API
    const an_thread_t thr = an_get_async(AN_DOG, &callback);

    // this will run while the thread is still requesting to the API
    printf("Hello, World!
");

    // wait for thread to terminate before we exit the main function
    an_thread_wait(thr);
    
    // global cleanup
    an_cleanup(NULL);
    
    return 0;
}
```
