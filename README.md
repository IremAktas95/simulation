# Simulation
# STM32 Kontrollü Röle Sürücü ve Akım Okuma Devresi

Bu proje, bir STM32 mikrodenetleyicisi ile 24V’luk bir rölenin güvenli bir şekilde sürülmesini ve bobin akımının op-amp tabanlı bir devre üzerinden ADC ile izlenmesini konu alan LTspice simülasyon çalışmasıdır.

## 1. Genel Bakış
Devre üç temel işlevi yerine getirir:
*   **Anahtarlama:** Düşük taraf (low-side) MOSFET sürücüsü ile 24V röle kontrolü.
*   **Koruma:** Flyback diyotu ile ters EMK baskılaması ve MOSFET güvenliği.
*   **Geri Okuma:** Bobin akımının shunt direnci üzerinden ölçülüp STM32 ADC girişine (0–3.3V) uygun seviyeye getirilmesi.

## 2. Devre Blokları ve Çalışma Prensibi
*   **MOSFET Sürücü:** STM32 GPIO pini, 2N7002 N-kanal MOSFET’i tetikler. Gate hattına eklenen 10kΩ pull-down direnci, STM32'nin reset anında rölenin istemsiz çekmesini engeller.
*   **Bobin ve Koruma:** Omron G5RL serisi gibi bir röle bobini 24V ile beslenir. Bobin enerjisi kesildiğinde oluşan yüksek gerilim, 1N5819 Schottky diyot ile sönümlenir.
*   **Durum Göstergesi:** Röle enerjili iken aktif olan bir LED, görsel geri bildirim sağlar.
*   **Akım İzleme:** Devrede, bobin akımını izlemek için 10Ω değerinde bir shunt direnci (R_{shunt}) kullanılmıştır. Bu değer, nominal bobin akımında (~16.7mA) yaklaşık 150-167mV seviyesinde bir gerilim düşümü oluşturacak şekilde seçilmiştir.

## 3. Malzeme Listesi
| Referans | Değer | Açıklama |
| :--- | :--- | :--- |
| **V1** | 24V | Ana Güç Kaynağı |
| **M2** | 2N7002 | N-Kanal MOSFET |
| **D1** | 1N5819HW | Flyback Diyodu |
| **R7** | 10Ω | Akım Algılama (Shunt) Direnci |
| **R2** | 10kΩ | Gate Pull-down Direnci |
| **U1** | INA199A1 | Op-Amp (Yükselteç) |

## 4. Akım ve ADC Hesaplamaları
Bobin akımının STM32 tarafından doğrusal olarak okunması için kazanç (gain) belirlenmiştir:
*   **Kazanç:** Gain = 1 + (R9/R8) = 11
*   **Çıkış Gerilimi:** Vout = Gain × (I_bobin × R_shunt)

## 5. Simülasyon ve SPICE Direktifleri
Çalışma parametrelerini test etmek için aşağıdaki komutlar kullanılmıştır:
*  .step param Rdeger 10 500 50` (Yük direncini tarama)
*  .step param Vtetik 2 5 1` (Tetikleme gerilimini tarama)
*  .tran 100m` (Zaman analizi)


---
