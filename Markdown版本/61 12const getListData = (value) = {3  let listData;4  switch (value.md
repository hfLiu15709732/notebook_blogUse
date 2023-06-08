```jsx

const getListData = (value) => {
  let listData;
  switch (value.date()) {
    case 8:
      listData = [
        {
          type: 'success',
          content: '收入：222 ￥',
        },
        {
          type: 'error',
          content: '支出：245 ￥',
        },
        {
          type: 'warning',
          content: '结余：-21￥',
        },
      ];
      break;
    case 10:
      listData = [
        {
          type: 'success',
          content: '收入：222 ￥',
        },
        {
          type: 'error',
          content: '支出：245 ￥',
        },
        {
          type: 'warning',
          content: '结余：-21￥',
        },
      ];
      break;
    case 15:
      listData = [
        {
          type: 'success',
          content: '收入：222 ￥',
        },
        {
          type: 'error',
          content: '支出：245 ￥',
        },
        {
          type: 'warning',
          content: '结余：-21￥',
        },
      ];
      break;
    default:
  }
  return listData || [];
};
const getMonthData = (value) => {
  if (value.month() === 8) {
    return 1394;
  }
};
```

