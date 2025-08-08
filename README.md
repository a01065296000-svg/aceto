# aceto
acetoremember
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from webdriver_manager.chrome import ChromeDriverManager
import time

driver = webdriver.Chrome(ChromeDriverManager().install())

# 1. 네이버 로그인 페이지 이동
driver.get("https://nid.naver.com/nidlogin.login")
time.sleep(2)

# 2. 아이디/비밀번호 입력
username = driver.find_element(By.ID, "id")
password = driver.find_element(By.ID, "pw")
username.send_keys("your_username")
password.send_keys("your_password")
password.send_keys(Keys.RETURN)

# 3. 로그인 후 대기 (웹드라이버가 로그인 후 리다이렉션 처리)
wait = WebDriverWait(driver, 10)
wait.until(EC.url_changes("https://nid.naver.com/nidlogin.login"))

# 4. 네이버 카페 게시글 페이지 이동
cafe_url = "https://cafe.naver.com/pythoneduc?iframe_url_utf8=%2FArticleRead.nhn%253Fclubid%3D31538535%2526articleid%3D3%2526menuid%3D2%2526boardtype%3DL"
driver.get(cafe_url)

# 5. iframe으로 전환
wait.until(EC.frame_to_be_available_and_switch_to_it("cafe_main"))

# 6. 게시글 제목 및 본문 읽기
title = wait.until(EC.presence_of_element_located((By.CSS_SELECTOR, ".tit-box h3"))).text
content = wait.until(EC.presence_of_element_located((By.CSS_SELECTOR, ".se-main-container"))).text

print("제목:", title)
print("본문:", content)

# 7. 브라우저 종료
driver.quit()
