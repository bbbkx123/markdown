# useState
```js
// 使用方法
const [count, setCount] = useState(0)
// 更新
setCount(count + 1)

// 对象数组类型
const [persons, setPersons] = useState([
  {
    name: 'kiana',
    age: 18
  }, {
    name: 'mei',
    age: 19
  }, {
    name: 'rita',
    age: 20
  },
])

// 更新对象数组类型数据
setPersons(persons => {
  persons.push({
    name: 'bronya',
    age: 17
  })
  // 使用浅拷贝(关键)
  return [...persons]
})
```

# useEffect

# useCallback