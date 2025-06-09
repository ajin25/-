class Drink:
    def __init__(self, name, price, stock):
        self.name = name
        self.price = price
        self.stock = stock

class VendingMachine:
    def __init__(self):
        self.drinks = {
            1: Drink("아이시스 8.0", 800, 5),
            2: Drink("아쿠아제로", 2000, 5),
            3: Drink("레몬워터", 1800, 5),
            4: Drink("옥수수 수염차", 1600, 5),
            5: Drink("콘트라베이스", 2200, 5),
            6: Drink("트레버", 1300, 5),
            7: Drink("펩시제로", 1100, 5),
            8: Drink("펩시", 1100, 5),
            9: Drink("칠성사이다 제로", 1300, 5),
            10: Drink("칠성사이다", 1300, 5),
            11: Drink("망고", 1200, 5),
            12: Drink("립톤", 1200, 5),
            13: Drink("사과에이드", 1100, 5),
            14: Drink("포도에이드", 1100, 5),
            15: Drink("가나초코", 900, 5),
            16: Drink("레쓰비", 900, 5),
            17: Drink("핫식스", 1300, 5),
            18: Drink("밀키스", 1100, 5),
            19: Drink("핫식스 제로", 1300, 5),
            20: Drink("카페타임", 1200, 5),
            21: Drink("게토레이", 1000, 5),
            22: Drink("코코팜", 1000, 5),
            23: Drink("식혜", 1000, 5),
        }
        self.money_inserted = 0
        self.coins = {1000: 10, 500: 10, 100: 10, 50: 10}  # 초기 거스름돈 재고
        self.admin_pw = "1234"

    def insert_money(self, amount):
        if amount in [1000, 500, 100, 50]:
            self.money_inserted += amount
            self.coins[amount] += 1
            print(f"\n현재 투입금액: {self.money_inserted}원")
        else:
            print("지원되지 않는 화폐입니다.")

    def show_menu(self):
        print("\n음료 메뉴:")
        for num, drink in self.drinks.items():
            print(f"{num}. {drink.name} - {drink.price}원 ({drink.stock}개 남음)")

    def select_drink(self, choice):
        if choice not in self.drinks:
            print("잘못된 선택입니다.")
            return
        drink = self.drinks[choice]
        if drink.stock == 0:
            print("해당 음료는 품절입니다.")
        elif self.money_inserted < drink.price:
            print("금액이 부족합니다.")
        else:
            drink.stock -= 1
            self.money_inserted -= drink.price
            print(f"{drink.name} 나왔습니다. 남은 금액: {self.money_inserted}원")

    def return_change(self):
        print("\n[거스름돈 반환]")
        change = self.money_inserted
        result = {}
        for coin in sorted(self.coins.keys(), reverse=True):
            count = min(change // coin, self.coins[coin])
            if count > 0:
                result[coin] = count
                change -= coin * count
                self.coins[coin] -= count
        if change == 0:
            for coin, cnt in result.items():
                print(f"{coin}원 x {cnt}")
            self.money_inserted = 0
        else:
            print("거스름돈 부족! 관리자에게 문의하세요.")

    def admin_mode(self):
        pw = input("관리자 비밀번호 입력: ")
        if pw != self.admin_pw:
            print("비밀번호 오류.")
            return
        print("\n[관리자 모드]")
        print("1. 재고 추가  2. 가격 변경  3. 종료")
        while True:
            cmd = input("선택: ")
            if cmd == "1":
                self.show_menu()
                choice = int(input("재고를 추가할 음료 번호: "))
                qty = int(input("추가할 수량: "))
                if choice in self.drinks:
                    self.drinks[choice].stock += qty
                    print("재고가 추가되었습니다.")
            elif cmd == "2":
                self.show_menu()
                choice = int(input("가격을 변경할 음료 번호: "))
                new_price = int(input("새 가격: "))
                if choice in self.drinks:
                    self.drinks[choice].price = new_price
                    print("가격이 변경되었습니다.")
            elif cmd == "3":
                print("관리자 모드 종료")
                break
            else:
                print("잘못된 선택입니다.")


def run():
    vm = VendingMachine()
    print("==== 자판기 프로그램 시작 ====")
    while True:
        print("\n1. 돈 투입  2. 음료 선택  3. 거스름돈 반환  4. 관리자 모드  5. 종료")
        action = input("선택: ")
        if action == "1":
            amount = int(input("금액 입력 (1000, 500, 100, 50만 가능): "))
            vm.insert_money(amount)
        elif action == "2":
            vm.show_menu()
            choice = int(input("음료 번호 선택: "))
            vm.select_drink(choice)
        elif action == "3":
            vm.return_change()
        elif action == "4":
            vm.admin_mode()
        elif action == "5":
            print("자판기 프로그램 종료")
            break
        else:
            print("잘못된 입력입니다.")


if __name__ == "__main__":
    run()
