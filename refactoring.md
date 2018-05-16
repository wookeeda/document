refactoring
=

Conditional Complexity Refactoring 
-

### 1) invert-if
<pre>
if ( !A ) {
    b 
} else {
    c
}
</pre>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;↓
<pre>
if (A) {
  C
} else {
  B
}
</pre>

### 2) split condition
<pre>
If (A || B) {
  C
} else {
  D
}
</pre>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;↓
<pre>
If (A) {
  C
} else if (B) {
  C
} else {
  D
}
</pre>


### 3) Add else block
<pre>
If (A) {
  B
}
</pre>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;↓
<pre>
If (A) {
  B
} else {
}
</pre>

### 4) Join if-statement with inner if-statement
<pre>
If (A) {
  if (B) {
    C
  }
}
</pre>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;↓
<pre>
If (A && B) {
  C
}
</pre>

### 5) Exchange conditions for inner and outer if-statement
<pre>
If (A) {
  if (B) {    C
  } else {
    D
  }
}
</pre>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;↓
<pre>
If (B) {
  if (A) {
    C
  }
} else {
  if (A) {
    D
  }
}
</pre>

### 6) Combine outer else and inner if-statement
<pre>
If (A) {
  B
} else {
  if (C) {
    D
  } else {
    E
  }
}
</pre>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;↓
<pre>
If (A) {
  B
} else if (C) {
  D
} else {
  E
}
</pre>

### 7) Consolidate Duplicate Conditional Fragments
<pre>
If (A) {
  B
  D
} else {
  C
  D
}
</pre>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;↓
<pre>
If (A) {
  B
} else {
  C
} 
D
</pre>

### 8) Duplicate Statements into if-statement
<pre>
If (A) {
  Z
  B
  D
} else {
  Z
  C
  D
}
</pre>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;↓
<pre>
Z

If (A) {
  B
} else {
  C
}

D
</pre>
