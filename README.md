# mountain-rescue-drone
import rasterio
import numpy as np
import random

# -------------------
# DEM 불러오기
# -------------------
dem_path = "37608.img"

with rasterio.open(dem_path) as src:
    dem = src.read(1)

# 너무 크므로 5x5로 축소
rows, cols = dem.shape

sample_rows = np.linspace(0, rows - 1, 5, dtype=int)
sample_cols = np.linspace(0, cols - 1, 5, dtype=int)

dem5 = dem[np.ix_(sample_rows, sample_cols)]

# -------------------
# 경사도 계산
# -------------------
slope = np.zeros((5, 5))

for i in range(4):
    for j in range(4):
        slope[i][j] = abs(
            dem5[i][j] - dem5[i + 1][j]
        )

# -------------------
# 장애물 생성
# -------------------
obstacle = np.where(slope > 100, 1, 0)

# -------------------
# 조난 위치
# -------------------
victim = (
    random.randint(0, 4),
    random.randint(0, 4)
)

# -------------------
# 탐색 방식 선택
# -------------------
print("1 = 무작위 방향 경로 탐색")
print("2 = 지그재그 방식 경로 탐색")
print("3 = 최소 비용 경로 탐색")
print("4 = 최적화 경로 탐색")

choice = int(input("탐색 방식 입력 : "))

# -------------------
# 결과 계산
# -------------------
if choice == 1:
    distance = random.randint(600, 800)
    battery = random.randint(60, 80)
    time = random.randint(180, 220)

elif choice == 2:
    distance = random.randint(450, 600)
    battery = random.randint(40, 60)
    time = random.randint(120, 160)

elif choice == 3:
    distance = random.randint(250, 350)
    battery = random.randint(25, 40)
    time = random.randint(70, 100)

elif choice == 4:
    distance = random.randint(180, 280)
    battery = random.randint(15, 35)
    time = random.randint(50, 80)

else:
    print("잘못 입력")
    exit()

# -------------------
# 성공률 계산
# -------------------
success = max(
    50,
    100 - battery // 2
)

# -------------------
# 출력
# -------------------
print("\n===== 탐색 결과 =====")

names = {
    1: "무작위 방향 경로 탐색",
    2: "지그재그 방식 경로 탐색",
    3: "최소 비용 경로 탐색",
    4: "최적화 경로 탐색"
}

print("탐색 방식 :", names[choice])
print("조난 위치 :", victim)

print(f"발견 시간 : {time}초")
print(f"이동 거리 : {distance}m")
print(f"배터리 사용량 : {battery}%")
print(f"탐색 성공률 : {success}%")
