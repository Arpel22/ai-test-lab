from typing import Union, Optional
import logging

# Налаштування логування для продакшн-моніторингу
logger = logging.getLogger(__name__)

def safe_calc(a: Union[int, float], b: Union[int, float]) -> Optional[float]:
    """
    Виконує безпечне ділення двох чисел з обробкою винятків.
    
    Args:
        a: Чисельник.
        b: Знаменник.
        
    Returns:
        Результат ділення або None, якщо виникла помилка.
    ```python
    """
    try:
        # Валідація типів (Best practice для Senior)
        if not all(isinstance(x, (int, float)) for x in [a, b]):
            raise TypeError("Аргументи повинні бути числами")
            
        return float(a / b)
        
    except ZeroDivisionError:
        logger.error("Спроба ділення на нуль!")
        return None
    except TypeError as e:
        logger.error(f"Помилка типів: {e}")
        return None
    except Exception as e:
        logger.critical(f"Непередбачена помилка: {e}")
        return None

# Як запустити тест:
if __name__ == "__main__":
    print(f"Успіх: {safe_calc(10, 2)}")   # 5.0
    print(f"Помилка: {safe_calc(10, 0)}")  # None
