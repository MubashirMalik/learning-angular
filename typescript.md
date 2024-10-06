
### Generics

```typescript
function insertAtBeginning<T>(array: T[], value: T) {
    const newArray = [...array, value];
    return newArray;
}

const numberArray = insertAtBeginning([1, 2, 3], 0);
// numberArray[0].split('');

const stringArray = insertAtBeginning(['a', 'b', 'c'], 'd');
```

### Classes
- Blueprints for objects

### Interfaces
- Only exist in TypeScript and not in JavaScript
- Same as type alias but with a extra feature
- Interfaces can be implemented by classes
- Angular has some interfaces that force you to implement certain methods in classes like OnInit
- I think interfaces are like <mark>Abstract classes</mark> in C++
