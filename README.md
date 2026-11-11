# Crypto Coders | Solidity Native ETH Vesting Wallet

قرارداد هوشمند کیف پول وستینگ (Vesting Wallet) بر پایه Native ETH به زبان Solidity همراه با دوره تنفس (Cliff) و آزادسازی خطی زمان‌بندی‌شده.

## ساختار فایل‌ها و قرارداد

| فایل | نام قرارداد | نسخه کامپایلر | توضیحات |
|---|---|---|---|
| `Vesting.sol` | `VestingWallet` | `^0.8.24` | مدیریت قفل و آزادسازی تدریجی ETH برای ذی‌نفع با قابلیت لغو توسط مالک |

## متغیرهای حالت (State Variables)
- `owner`: آدرس مالک/سرمایه‌گذار ایجادکننده قرارداد
- `user`: آدرس ذی‌نفع (دریافت‌کننده ETH آزادشده)
- `start`: زمان شروع دوره وستینگ (Unix Timestamp)
- `cliff`: مدت دوره تنفس (Cliff) بر حسب ثانیه
- `duration`: مدت کل دوره وستینگ بر حسب ثانیه
- `totalAmount`: کل مقدار ETH قفل‌شده در زمان استقرار قرارداد
- `claimed`: مجموع ETH برداشت‌شده توسط ذی‌نفع تا این لحظه
- `canceled`: وضعیت لغو شدن قرارداد توسط مالک
- `vestedAtCancel`: مقدار ETH آزادشده برای ذی‌نفع تا لحظه لغو

## توابع اصلی
- `constructor(address _user, uint256 _start, uint256 _cliff, uint256 _duration) payable`: دریافت ETH اولیه و تنظیم پارامترهای زمان‌بندی و ذی‌نفع
- `vested()`: محاسبه کل مقدار ETH آزادشده تا لحظه فعلی بر اساس دوره Cliff و آزادسازی خطی
- `available()`: مشاهده مقدار ETH قابل برداشت فعلی (`vested() - claimed`)
- `claim()`: برداشت ETH آزادشده توسط ذی‌نفع (`onlyUser`) با محافظت در برابر Reentrancy (`noReentry`)
- `cancel()`: لغو ادامه وستینگ توسط مالک (`onlyOwner`)، تثبیت سهم آزادشده ذی‌نفع و بازگشت مابقی ETH آزادنشده به مالک
- `cliffTime()`، `endTime()`، `totalClaimed()`، `remaining()` و `balance()`: توابع مشاهده وضعیت زمانی و مالی قرارداد
