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

# useReducer

# useContext
# useRef

# useEffect

# useCallback

## 自定义useWatch
```ts
import { useEffect, useRef } from 'react';

type CallBack<T> = (prev: T | undefined) => void;
type Config = { immediate: boolean };

/**
 * useWatch
 * @param dep 监听对象
 * @param callback
 * @param config 监听设置
 */

function useWatch<T>(
  dep: T,
  callback: CallBack<T>,
  config: Config = { immediate: false },
) {
  // 监听禁止标志
  const stop = useRef<boolean>(false);
  // 初始化标志
  const inited = useRef<boolean>(false);
  const prev = useRef<T>();
  const { immediate } = config;

  useEffect(() => {
    if (!stop.current) {
      const execute = () => callback(prev.current);
      // 防止immediate为false时, 首次加载就执行
      if (!inited.current) {
        inited.current = true;
        if (immediate) {
          execute();
        }
      } else {
        execute();
      }
      prev.current = dep;
    }
  }, [dep]);

  return () => {
    stop.current = true;
  };
}

export { useWatch };

```