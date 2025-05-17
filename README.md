#include <iostream>
#include <cassert>
 
// Función para convertir caracteres a mayúsculas o reemplazar '/' por '\'
static char upper_backslash(char c) {
    return c == '/' ? '\\' : toupper(c);
}
 
// Tabla de hash predefinida
static uint32_t const s_hashtable[16] = {
    0x486E26EE, 0xDCAA16B3, 0xE1918EEF, 0x202DAFDB,
    0x341C7DC7, 0x1C365303, 0x40EF2D37, 0x65FD5E49,
    0xD6057177, 0x904ECE93, 0x1C38024F, 0x98FD323B,
    0xE3061AE7, 0xA39B0FA1, 0x9797F25F, 0xE4444563,
};
 
// Función de hash personalizada
uint32_t SStrHash(const char* string, bool no_caseconv, uint32_t seed) {
    assert(string);
 
    if (!seed) {
        seed = 0x7FED7FED;
    }
 
    uint32_t shift = 0xEEEEEEEE;
    while (*string) {
        char c = *string++;
        if (!no_caseconv) {
            c = upper_backslash(c);
        }
 
        seed = (s_hashtable[c >> 4] - s_hashtable[c & 0xF]) ^ (shift + seed);
        shift = c + seed + 33 * shift + 3;
    }
 
    return seed ? seed : 1;
}
 
int main() {
    // Llamada a la función y salida del resultado
    const char* input = "spellmisc";
    uint32_t hash_result = SStrHash(input, false, 0);
    std::cout << "Hash result for '" << input << "': " << hash_result << std::endl;
 
    return 0;
}
