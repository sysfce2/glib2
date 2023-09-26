Title: Data Structures

# Data Structures

GLib includes a number of basic data sructures, such as linked lists, hash tables,
arrays, queues, etc.

## Arrays

GLib arrays ([struct@GLib.Array]) are similar to standard C arrays, except that they grow
automatically as elements are added.

Array elements can be of any size (though all elements of one array are the same size),
and the array can be automatically cleared to '0's and zero-terminated.

To create a new array use [method@Glib.Array.new].

To add elements to an array with a cost of O(n) at worst, use
[methodd@GLib.Array.append_val],
[method@GLib.Array.append_vals],
[method@GLib.Array.prepend_val],
[method@GLib.Array.prepend_vals],
[method@GLib.Array.insert_val] and
[method@GLib.Array.insert_vals].

To access an element of an array in O(1) (to read it or to write it),
use [method@GLib.Array.index].

To set the size of an array, use [method@GLib.Arrray.set_size].

To free an array, use [method@GLib.Array.unref] or [method@GLib.Array.free].

All the sort functions are internally calling a quick-sort (or similar)
function with an average cost of O(n log(n)) and a worst case cost of O(n^2).

Here is an example that stores integers in a [struct@GLib.Array]:

```c
GArray *array;
int i;

// We create a new array to store int values.
// We don't want it zero-terminated or cleared to 0's.
array = g_array_new (FALSE, FALSE, sizeof (int));

for (i = 0; i < 10000; i++)
  {
    g_array_append_val (array, i);
  }

for (i = 0; i < 10000; i++)
  {
    if (g_array_index (array, int, i) != i)
      g_print ("ERROR: got %d instead of %d\n",
               g_array_index (array, int, i), i);
  }

g_array_free (array, TRUE);
```

## Pointer Arrays

Pointer Arrays ([struct@GLib.PtrArray]) are similar to Arrays but are used
only for storing pointers.

If you remove elements from the array, elements at the end of the
array are moved into the space previously occupied by the removed
element. This means that you should not rely on the index of particular
elements remaining the same. You should also be careful when deleting
elements while iterating over the array.

To create a pointer array, use [method@GLib.PtrArray.new].

To add elements to a pointer array, use [method@GLib.PtrArray.add].

To remove elements from a pointer array, use
[method@GLib.PtrArray.remove],
[method@GLib.PtrArray.remove_index] or
[method@GLib.PtrArray.remove_index_fast].

To access an element of a pointer array, use [method@GLib.PtrArray.index].

To set the size of a pointer array, use [method@GLib.PtrArray.set_size].

To free a pointer array, use [method@GLib.PtrArray.unref] or [method@GLib.PtrArray.free].

An example using a [struct@Glib.PtrArray]:

```c
GPtrArray *array;
char *string1 = "one";
char *string2 = "two";
char *string3 = "three";

array = g_ptr_array_new ();
g_ptr_array_add (array, (gpointer) string1);
g_ptr_array_add (array, (gpointer) string2);
g_ptr_array_add (array, (gpointer) string3);

if (g_ptr_array_index (array, 0) != (gpointer) string1)
  g_print ("ERROR: got %p instead of %p\n",
           g_ptr_array_index (array, 0), string1);

g_ptr_array_free (array, TRUE);
```

## Byte Arrays

{struct@Glib.ByteArray] is a mutable array of bytes based on [struct@Glib.Array],
to provide arrays of bytes which grow automatically as elements are added.

To create a new `GByteArray` use [method@GLib.ByteArray.new].

To add elements to a `GByteArray`, use
[method@GLib.ByteArray.append] and [method@GLib.ByteArray.prepend].

To set the size of a `GByteArray`, use [method@GLib.ByteArray.set_size].

To free a `GByteArray`, use [method@Glib.ByteArray.unref] or [method@Glib.ByteArray.free].

An example for using a `GByteArray`:

```c
GByteArray *array;
int i;

array = g_byte_array_new ();
for (i = 0; i < 10000; i++)
  {
    g_byte_array_append (array, (guint8*) "abcd", 4);
  }

for (i = 0; i < 10000; i++)
  {
    g_assert (array->data[4*i] == 'a');
    g_assert (array->data[4*i+1] == 'b');
    g_assert (array->data[4*i+2] == 'c');
    g_assert (array->data[4*i+3] == 'd');
  }

g_byte_array_free (array, TRUE);
```

See [struct@Glib.Bytes] if you are interested in an immutable object representing a
sequence of bytes.
