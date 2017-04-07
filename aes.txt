
import numpy as np
import re
from functools import reduce

# Set the result to be output in hexidecimal
np.set_printoptions(formatter={'int':lambda x: '{:x}'.format(x)})

def stringToByteMatrix(string):
    a = [int('0x'+val, 0) for val in re.findall('.{1,2}', string)]
    return np.reshape(np.matrix(a), (4,4)).swapaxes(0,1) 

def stringToByteArray(string):
    return np.array([int('0x'+val, 0) for val in re.findall('.{1,2}', string)])

dataFile = open('aesinput.txt', 'r')
iterations = int(dataFile.readline())
rounds = int(dataFile.readline())
key = dataFile.readline()
plaintext = dataFile.readline()

sBox = np.matrix([[int('0x63',0), int('0x7C',0), int('0x77',0), int('0x7B',0), int('0xF2',0), int('0x6B',0), int('0x6F',0), int('0xC5',0), int('0x30',0), int('0x01',0), int('0x67',0), int('0x2B',0), int('0xFE',0), int('0xD7',0), int('0xAB',0), int('0x76',0)],
                    [int('0xCA',0), int('0x82',0), int('0xC9',0), int('0x7D',0), int('0xFA',0), int('0x59',0), int('0x47',0), int('0xF0',0), int('0xAD',0), int('0xD4',0), int('0xA2',0), int('0xAF',0), int('0x9C',0), int('0xA4',0), int('0x72',0), int('0xC0',0)],
                    [int('0xB7',0), int('0xFD',0), int('0x93',0), int('0x26',0), int('0x36',0), int('0x3F',0), int('0xF7',0), int('0xCC',0), int('0x34',0), int('0xA5',0), int('0xE5',0), int('0xF1',0), int('0x71',0), int('0xD8',0), int('0x31',0), int('0x15',0)],
                    [int('0x04',0), int('0xC7',0), int('0x23',0), int('0xC3',0), int('0x18',0), int('0x96',0), int('0x05',0), int('0x9A',0), int('0x07',0), int('0x12',0), int('0x80',0), int('0xE2',0), int('0xEB',0), int('0x27',0), int('0xB2',0), int('0x75',0)],
                    [int('0x09',0), int('0x83',0), int('0x2C',0), int('0x1A',0), int('0x1B',0), int('0x6E',0), int('0x5A',0), int('0xA0',0), int('0x52',0), int('0x3B',0), int('0xD6',0), int('0xB3',0), int('0x29',0), int('0xE3',0), int('0x2F',0), int('0x84',0)],
                    [int('0x53',0), int('0xD1',0), int('0x00',0), int('0xED',0), int('0x20',0), int('0xFC',0), int('0xB1',0), int('0x5B',0), int('0x6A',0), int('0xCB',0), int('0xBE',0), int('0x39',0), int('0x4A',0), int('0x4C',0), int('0x58',0), int('0xCF',0)],
                    [int('0xD0',0), int('0xEF',0), int('0xAA',0), int('0xFB',0), int('0x43',0), int('0x4D',0), int('0x33',0), int('0x85',0), int('0x45',0), int('0xF9',0), int('0x02',0), int('0x7F',0), int('0x50',0), int('0x3C',0), int('0x9F',0), int('0xA8',0)],
                    [int('0x51',0), int('0xA3',0), int('0x40',0), int('0x8F',0), int('0x92',0), int('0x9D',0), int('0x38',0), int('0xF5',0), int('0xBC',0), int('0xB6',0), int('0xDA',0), int('0x21',0), int('0x10',0), int('0xFF',0), int('0xF3',0), int('0xD2',0)],
                    [int('0xCD',0), int('0x0C',0), int('0x13',0), int('0xEC',0), int('0x5F',0), int('0x97',0), int('0x44',0), int('0x17',0), int('0xC4',0), int('0xA7',0), int('0x7E',0), int('0x3D',0), int('0x64',0), int('0x5D',0), int('0x19',0), int('0x73',0)],
                    [int('0x60',0), int('0x81',0), int('0x4F',0), int('0xDC',0), int('0x22',0), int('0x2A',0), int('0x90',0), int('0x88',0), int('0x46',0), int('0xEE',0), int('0xB8',0), int('0x14',0), int('0xDE',0), int('0x5E',0), int('0x0B',0), int('0xDB',0)],
                    [int('0xE0',0), int('0x32',0), int('0x3A',0), int('0x0A',0), int('0x49',0), int('0x06',0), int('0x24',0), int('0x5C',0), int('0xC2',0), int('0xD3',0), int('0xAC',0), int('0x62',0), int('0x91',0), int('0x95',0), int('0xE4',0), int('0x79',0)],
                    [int('0xE7',0), int('0xC8',0), int('0x37',0), int('0x6D',0), int('0x8D',0), int('0xD5',0), int('0x4E',0), int('0xA9',0), int('0x6C',0), int('0x56',0), int('0xF4',0), int('0xEA',0), int('0x65',0), int('0x7A',0), int('0xAE',0), int('0x08',0)],
                    [int('0xBA',0), int('0x78',0), int('0x25',0), int('0x2E',0), int('0x1C',0), int('0xA6',0), int('0xB4',0), int('0xC6',0), int('0xE8',0), int('0xDD',0), int('0x74',0), int('0x1F',0), int('0x4B',0), int('0xBD',0), int('0x8B',0), int('0x8A',0)],
                    [int('0x70',0), int('0x3E',0), int('0xB5',0), int('0x66',0), int('0x48',0), int('0x03',0), int('0xF6',0), int('0x0E',0), int('0x61',0), int('0x35',0), int('0x57',0), int('0xB9',0), int('0x86',0), int('0xC1',0), int('0x1D',0), int('0x9E',0)],
                    [int('0xE1',0), int('0xF8',0), int('0x98',0), int('0x11',0), int('0x69',0), int('0xD9',0), int('0x8E',0), int('0x94',0), int('0x9B',0), int('0x1E',0), int('0x87',0), int('0xE9',0), int('0xCE',0), int('0x55',0), int('0x28',0), int('0xDF',0)],
                    [int('0x8C',0), int('0xA1',0), int('0x89',0), int('0x0D',0), int('0xBF',0), int('0xE6',0), int('0x42',0), int('0x68',0), int('0x41',0), int('0x99',0), int('0x2D',0), int('0x0F',0), int('0xB0',0), int('0x54',0), int('0xBB',0), int('0x16',0)]])

columnMixer = np.matrix('2 3 1 1; 1 2 3 1; 1 1 2 3; 3 1 1 2')

state = stringToByteMatrix(plaintext)
keyArray = stringToByteArray(key)

# Performs a circular shift by an amount
def rotate(array, amount):
    return np.roll(array, amount)

# Shifts each row to the left based on the index of the row
def shiftRows(matrix):
    newState = matrix
    for rowNumber in range(len(newState)):
        newState[rowNumber] = rotate(newState[rowNumber], -rowNumber)
    return newState

# Performs a lookup in the sBox table for all values in a matrix or array
def subBytes(matrix):
    rowLookup = matrix & 0b1111
    columnLookup = matrix >> 4
    return np.vectorize(lambda a, b: sBox[a, b])(columnLookup, rowLookup)

# Performs a multiplication in the Galois Field
# Explanation and helpful example courtesy of http://www.samiam.org/rijndael.html
def gmul(a, b):
    p = 0
    for i in range(8):
        if (b & 1) == 1:
            p ^= a
        isHigh = (a & 0x80)
        a <<= 1
        if isHigh:
            # polynomial
            a ^= 0x1b
        b >>= 1
    return p

# Finds the rcon value of a number
# Explanation and helpful example courtesy of http://www.samiam.org/rijndael.html
def rcon(val):
    c = 1
    if val is 0:
        return 0
    while val is not 1:
        c = gmul(c, 2)
        val -= 1
    return c & 255


def mixColumns(state):
    state = state.transpose()
    return np.matrix([gmulColumns(state[i], i) for i in range(len(state))]).transpose()
    
# Performs the Galois Field multiplication for a given column
# Flattens the array then performs gmul on each element, then reduces the result to get a single number
def gmulColumns(row, i):
    clone = np.copy(row)
    return [reduce((lambda x, y: (x ^ y) & 255), np.vectorize(gmul)(clone, np.array(columnMixer[j])).flatten()) for j in range(4)]

# The core part of the key scheduling
# Rotates and XORs the key with the rcon for confusion
def scheduleCore(key, i):
    key = subBytes(rotate(key, -1))
    key[0] ^= rcon(i)
    return key

# Expands the key depending on the number of rounds
def keyExpansion(key, rounds):
    c = 16
    rcon = 1
    BYTE_KEY_SIZE = 16 * (rounds)
    
    # Values stored here in hewKey
    newKey = np.zeros(BYTE_KEY_SIZE, dtype=np.uint8)
    newKey[0:16] = key
    temp = np.zeros(4, dtype=np.uint8)
    
    while c < BYTE_KEY_SIZE:
        # Temp is last 4 elements
        temp = newKey[c-4:c]
        
        if c % 16 is 0:
            temp = scheduleCore(temp, rcon)
            rcon += 1
        
        newKey[c:c+4] = newKey[c-16:c-12] ^ temp
        c += 4
        
    return np.reshape(newKey, (rounds, 16))
    
# XOR the generated round key from the key expansion with the current matrix state
def addRoundKey(state, keyList, rnd):
    return np.matrix(state ^ np.reshape(keyList[rnd], (4, 4)).transpose())

expandedKeyList = keyExpansion(keyArray, rounds + 1)
previousState = state

for i in range(iterations):
    # Initial XOR with roundkey
    state = addRoundKey(state, expandedKeyList, 0);

    # Main part of loop
    for rnd in range(1, rounds):
        state = subBytes(state)
        state = shiftRows(state)
        state = mixColumns(state)
        state = addRoundKey(state, expandedKeyList, rnd)

    # On th efinal loop, exclude the mix columns
    state = subBytes(state)
    state = shiftRows(state)
    state = addRoundKey(state, expandedKeyList, rounds)
    
    # XOR with prior plaintext if not the last iteration
    if i is not iterations - 1:
        t = state
        state ^= previousState
        previousState = t

# Transpose and reshape the final matrix
# Output the result
finalCipherText = np.reshape(state.transpose(), (1, -1))
print('Final Ciphertext', finalCipherText)
