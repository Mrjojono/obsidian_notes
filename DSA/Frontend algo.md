


```typescript
function sum_char_code(n: string): number {
    let sum = 0;
    for (let i = 0; i < n.length; i++) {
        for (let j = 0; j < n.length; j++) {
        sum += n.charCodeAt(i);
    }
    }
    return sum;
}

console.log(sum_char_code("joan")); 


```



