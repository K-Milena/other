# other

uint8_t SPI_Read_Register(uint8_t reg_address)
{
    uint8_t tx_data[2];
    uint8_t rx_data[2];

    // Bit 7 = 1 oznacza operację odczytu
    tx_data[0] = reg_address | 0x80; // Ustawienie bitu odczytu
    tx_data[1] = 0x00;              // Dummy byte

    // Aktywuj CS (niski stan logiczny)
    HAL_GPIO_WritePin(GPIOA, GPIO_PIN_4, GPIO_PIN_RESET);

    // Wysyłanie i odbieranie danych
    HAL_SPI_TransmitReceive(&hspi1, tx_data, rx_data, 2, HAL_MAX_DELAY);

    // Dezaktywuj CS (wysoki stan logiczny)
    HAL_GPIO_WritePin(GPIOA, GPIO_PIN_4, GPIO_PIN_SET);

    return rx_data[1]; // Odczytana wartość
}


uint8_t SPI_Read_Register(uint8_t reg_address)
{
    uint8_t tx_data[2];
    uint8_t rx_data[2];

    // Bit 7 = 1 oznacza operację odczytu
    tx_data[0] = reg_address | 0x80; // Ustawienie bitu odczytu
    tx_data[1] = 0x00;              // Dummy byte

    // Aktywuj CS (niski stan logiczny)
    HAL_GPIO_WritePin(GPIOA, GPIO_PIN_4, GPIO_PIN_RESET);

    // Wysyłanie i odbieranie danych
    HAL_SPI_TransmitReceive(&hspi1, tx_data, rx_data, 2, HAL_MAX_DELAY);

    // Dezaktywuj CS (wysoki stan logiczny)
    HAL_GPIO_WritePin(GPIOA, GPIO_PIN_4, GPIO_PIN_SET);

    return rx_data[1]; // Odczytana wartość
}

void SPI_Write_Register(uint8_t reg_address, uint8_t value)
{
    uint8_t tx_data[2];

    // Bit 7 = 0 oznacza operację zapisu
    tx_data[0] = reg_address & 0x7F; // Ustawienie bitu zapisu
    tx_data[1] = value;

    // Aktywuj CS (niski stan logiczny)
    HAL_GPIO_WritePin(GPIOA, GPIO_PIN_4, GPIO_PIN_RESET);

    // Wysyłanie danych
    HAL_SPI_Transmit(&hspi1, tx_data, 2, HAL_MAX_DELAY);

    // Dezaktywuj CS (wysoki stan logiczny)
    HAL_GPIO_WritePin(GPIOA, GPIO_PIN_4, GPIO_PIN_SET);
}

te poniżej w main


SPI_Write_Register(0x20, 0x0F); // CTRL_REG1: Włącz czujnik i odczyty


int8_t Read_Temperature_A3G4250D(void)
{
    uint8_t temp = SPI_Read_Register(0x26); // OUT_TEMP
    return (int8_t)temp; // OUT_TEMP jest 8-bitowy, rzutujemy na znakowy int
}

while (1)
{
    int8_t temperature = Read_Temperature_A3G4250D();
    printf("Temperature: %d°C\n", temperature);

    HAL_Delay(1000); // Opóźnienie 1 sekunda
}

