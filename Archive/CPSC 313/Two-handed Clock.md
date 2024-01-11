# Two-handed Clock
- writeback hand is a fixed distance ahead of the replacement hand
- whenever a dirty page is found, send it to disk controller for writeback
- writes set both the use and dirty bits

## Example
Assume that memory holds only 4 pages, numbered 0 to 3. The following table lists the pages, in-order, starting at 0 and shows the virtual page number (with the current state of their use and dirty bits) currently resident in the given physical page.

The replacement hand is initially at position 0. The write hand is initially at position 2.

On every eviction, you should:

First attempt to write one dirty page back. It should first consider the page under the dirty hand and continue looking from there; if it does not find a dirty page to evict, the hand should return to the position at which it started. If it does find a page to evict, leave the hand on the location that was just written.
Second, you should select a page for eviction, using the algorithm as presented in the pre-class work.
In other words, you should move the write hand before you move the replacement hand.
In a real system, the tricky part of this algorithm is keeping the write hand moving at 'the right' rate. You should ignore this. It is fine if your write hand passes your replacement hand or if your replacement hand passes your write hand. For example, if there are no dirty pages to write back, then the write hand will end up in exactly the same position where it started (and will have passed the replacement hand).

Some corner cases:

It is possible that the replacement hand will want to replace a page that is dirty. Assume that you do just that, write the page you are about to replace and then read the new page in. Leave the clock hand in the slot that you just replaced.
When replcaement causes a page to be written, advance the write hand to the next slot.
Fill in the table below with the correct page numbers and use bits after the following operations have been performed.

In each operation, the hex value is a virtual page number; enter virtual page numbers exactly as they appear in the reference stream (cut and paste is your friend). Enter 0 or 1 for the use and dirty bits.

write 0x03, read 0xe0, read 0xa2, write 0x9e, write 0x23, read 0x69

initial

| Hands | Virtual page number | Use | Dirty |
| ----- | ------------------- | --- | ----- |
| RH    | 0x4a                | 1   | 0     |
|       | 0x03                | 0   | 0     |
| WH    | 0x23                | 1   | 0     |
|       | 0xa2                | 1   | 1     |

write 0x03

| Hands | Virtual page number | Use | Dirty |
| ----- | ------------------- | --- | ----- |
| RH    | 0x4a                | 1   | 0     |
|       | 0x03                | 1   | 1     |
| WH    | 0x23                | 1   | 0     |
|       | 0xa2                | 1   | 1     |

read 0xe0

| Hands | Virtual page number | Use | Dirty |
| ----- | ------------------- | --- | ----- |
| RH    | 0xe0                | 1   | 0     |
|       | 0x03                | 0   | 1     |
|       | 0x23                | 0   | 0     |
| WH    | 0xa2                | 0   | 0     |

read 0xa2

| Hands | Virtual page number | Use | Dirty |
| ----- | ------------------- | --- | ----- |
| RH    | 0xe0                | 1   | 0     |
|       | 0x03                | 0   | 1     |
|       | 0x23                | 0   | 0     |
| WH    | 0xa2                | 1   | 0     |

write 0x9e

| Hands | Virtual page number | Use | Dirty |
| ----- | ------------------- | --- | ----- |
|       | 0xe0                | 0   | 0     |
| RH/WH | 0x9e                | 1   | 1     |
|       | 0x23                | 0   | 0     |
|       | 0xa2                | 1   | 0     |

write 0x23

| Hands | Virtual page number | Use | Dirty |
| ----- | ------------------- | --- | ----- |
|       | 0xe0                | 0   | 0     |
| RH/WH | 0x9e                | 1   | 1     |
|     | 0x23                | 1   | 1     |
|       | 0xa2                | 1   | 0     |

read 0x69

| Hands | Virtual page number | Use | Dirty |
| ----- | ------------------- | --- | ----- |
| RH    | 0x69                | 1   | 0     |
| WH    | 0x9e                | 0   | 0     |
|       | 0x23                | 0   | 1     |
|       | 0xa2                | 0   | 0     |

### example 2
write 0xb9, write 0x2e, read 0xf0, read 0xb9, read 0x2e, write 0x7f

initial

| Hands | Virtual page number | Use | Dirty |
| ----- | ------------------- | --- | ----- |
|       | 0x65                | 1   | 1     |
| WH    | 0xbe                | 0   | 0     |
|       | 0xb9                | 1   | 0     |
| RH    | 0x1b                | 1   | 1     |

write 0xb9

| Hands | Virtual page number | Use | Dirty |
| ----- | ------------------- | --- | ----- |
|       | 0x65                | 1   | 1     |
| WH    | 0xbe                | 0   | 0     |
|       | 0xb9                | 1   | 1     |
| RH    | 0x1b                | 1   | 1     |

write 0x2e

| Hands | Virtual page number | Use | Dirty |
| ----- | ------------------- | --- | ----- |
|       | 0x65                | 0   | 1     |
| RH    | 0x2e                | 1   | 0     |
| WH    | 0xb9                | 1   | 0     |
|       | 0x1b                | 0   | 1     |

read 0xf0

| Hands | Virtual page number | Use | Dirty |
| ----- | ------------------- | --- | ----- |
|       | 0x65                | 0   | 1     |
|       | 0x2e                | 0   | 0     |
|       | 0xb9                | 0   | 0     |
| RH/WH | 0xf0                | 1   | 0     |

read 0xb9

| Hands | Virtual page number | Use | Dirty |
| ----- | ------------------- | --- | ----- |
|       | 0x65                | 0   | 1     |
|       | 0x2e                | 0   | 0     |
|       | 0xb9                | 1   | 0     |
| RH/WH | 0xf0                | 1   | 0     |

read 0x2e

| Hands | Virtual page number | Use | Dirty |
| ----- | ------------------- | --- | ----- |
|       | 0x65                | 0   | 1     |
|       | 0x2e                | 1   | 0     |
|       | 0xb9                | 1   | 0     |
| RH/WH | 0xf0                | 1   | 0     |

write 0x7f

| Hands | Virtual page number | Use | Dirty |
| ----- | ------------------- | --- | ----- |
| RH/WH | 0x7f                | 1   | 1     |
|       | 0x2e                | 1   | 0     |
|       | 0xb9                | 1   | 0     |
|       | 0xf0                | 0   | 0     |
