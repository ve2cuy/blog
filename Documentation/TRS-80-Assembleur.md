# TRS-80 : Assembleur

*Document converti depuis [ve2cuy.com/blog](https://ve2cuy.com/blog/index.php/trs-80-assembleur/) — dernière modification : 2025-11-25*

---

- Compilation, à partir de Windows d’un programme Z80 ([zmac](http://48k.ca/zmac.html)):

```
zmac file_name
```

- Tester un programme à partir d’un émulateur ([trs80gp](http://48k.ca/trs80gp.html)):

```
trs80gp -ld -m3 -vs zout/file_name ; -ld = LDOS, -vs = Sharp Display
```

- Les outils de développement sont dans

```
Z:\0 - Documentation\Réseau et équipement maison\TRS-80\ASM-Tool-Chain>
```

---

- Outils images disque

[Virtual Disk Utilities – Ira Goldklang’s TRS-80 Revived Site](https://www.trs-80.com/wordpress/emulators/disk-utilities/) (**TRSTools**)

- Pour le **TRS-80 modèle 2**:

Les fichiers images de type **IMD** doivent être convertis en format **HFE** avec l’application **[HxCFloppyEmulator](https://hxc2001.com/download/floppy_drive_emulator/HxCFloppyEmulator_soft.zip)**.

---

- Exemple d’un programme – Hello World – utilisant des appels de LDOS:

```
; zmac file_name
; trs80gp -ld -m3 -vs zout/file_name ; -ld = LDOS, -vs = Sharp Display
        ORG     7000H

@DSPLY  EQU     4467H  ;LDOS 5.1/MOD3 - pg 6-66
@EXIT   EQU     402DH  ;pg 6-60
@KEY    EQU     0049H  ;
@TIME   EQU     3036H  ;446DH ; 3036H  PG 6-58
@DATE   EQU     3033H

VIDEO   EQU     3C00H
VSIZE   EQU     1024
ENTER   EQU     0DH
TEXT    DEFB    'TIME AND DATE VIA SYS-CALL @TIME, @DATE', ENTER
BUFFER  DEFB    '        ', ENTER
MAIN:

        LD      DE,VSIZE
        LD      HL, VIDEO
LOOP:
        LD      A,'X'
        LD      (HL),A
        INC     HL
        DEC     DE
        LD      A,D
        OR      E
        JP      NZ,LOOP
        CALL    @KEY        ; WAIT UNTIL KEY PRESSED
        LD      HL,TEXT
        CALL    @DSPLY

; GET TIME AND DISPLAY IT
        LD      HL,BUFFER
        CALL    @TIME
        LD      HL,BUFFER
        CALL    @DSPLY

; GET DATE AND DISPLAY IT
        LD      HL,BUFFER
        CALL    @DATE
        LD      HL,BUFFER
        CALL    @DSPLY

        CALL    @KEY        ; WAIT UNTIL KEY PRESSED
        CALL    @EXIT
        END     MAIN
```

---

Un autre exemple – Compter jusqu’à 0xFFFF

```
; Auteur: Alain Boudreault (VE2CUY)
; Date: 2025.08.10
; Description: Compter de 0 à 255 et afficher en DEC et de 0 à 0xFFFF en HEX
; Assembleur: zmac
; Emulateur: trs80gp
; --------------------------------------------------------------------------
; zmac file_name
; trs80gp -ld -m3 -vs zout/file_name ; -ld = LDOS, -vs = Sharp Display
; --------------------------------------------------------------------------

                ORG     7000H

@DSPLY          EQU     4467H  ;LDOS 5.1/MOD3 - pg 6-66
@EXIT           EQU     402DH  ;pg 6-60
@KEY            EQU     0049H  ;
@TIME           EQU     3036H  ;446DH ; 3036H  PG 6-58
@DATE           EQU     3033H

VIDEO           EQU     3C00H
VSIZE           EQU     1024
ENTER           EQU     0DH
LINE_FEED       EQU     0AH
AUTEUR          DEFB    LINE_FEED
                DEFB    '---------------------------------------------------',          LINE_FEED
                DEFB    'Programme: counter/cmd, par VE2CUY 2025.08.20',                LINE_FEED
                DEFB    'Ce programme utilise des sous-routines de LDOS 5.1',           LINE_FEED
                DEFB    '---------------------------------------------------',          ENTER

MSG1
                DEFB    'Compter de 0 a 255 en DEC, Enter pour commencer ...',          ENTER

MSG2            DEFB    LINE_FEED, LINE_FEED
                DEFB    '---------------------------------------------------',          LINE_FEED
                DEFB    'Compter de 0 a 0xFFFF en HEX, Enter pour commencer:',          LINE_FEED
                DEFB    '---------------------------------------------------',          ENTER
BUFFER          DEFB    '        ', ENTER
TEXTE           DEFS    7       ; Buffer for number string'

; ---------------------------------------------------------------------
MAIN:
        LD      HL,AUTEUR
        CALL    @DSPLY
        LD      HL,MSG1
        CALL    @DSPLY
        CALL    @KEY            ; WAIT UNTIL KEY PRESSED

; Afficher les nombres de 0 à 255
Start:
        LD      IX, 0           ; IX = compteur de 0 à 255
boucle:
        LD      A, IXl          ; Charger la valeur du compteur dans A
        LD      HL,TEXTE
        CALL    NumberToString  ; Convert number in A to string at HL
        LD      HL,TEXTE        ; Now HL points to the end of the string, we need to backtrack to the start
        CALL    @DSPLY          ; Display the string
        INC     IX
        LD      A, IXl
        CP      0               ; quand IXl revient à 0, on a atteint 256
        JR      NZ, boucle
; ### FIN de : ; Afficher les nombres de 0 à 255

; Afficher les nombres de 0 à 0xFFFF en HEX
        LD      HL,MSG2
        CALL    @DSPLY
        CALL    @KEY            ; WAIT UNTIL KEY PRESSED
        LD      IX, 0           ; IX = compteur de 0 à 255
        LD      DE, 0

boucle2:
        LD      HL,TEXTE
        CALL    NumberToHex     ; Convert number in DE to string at HL
        PUSH    DE              ; ATTENTION, @DSPLY va modifier DE
        LD      HL,TEXTE        ; Now HL points to the end of the string, we need to backtrack to the start
        CALL    @DSPLY          ; Display the string
        POP     DE
        INC     DE
        INC     IX
                                ; Vérifier si IX == 0
                                ; Le Z80 ne permet pas de faire CP IX, 0 directement car
                                ; il ne supporte pas les comparaisons 16 bits avec un immédiat.
                                ; Il faut comparer chaque partie séparément.
        LD A, IXH
        OR IXL                  ; combine les deux : A sera 0 seulement si les deux sont 0
        CP 0                    ; pas nécessaire, mais explicite
        JR      NZ, boucle2
; ### FIN de : ; Afficher les nombres de 0x0 à 0xFFFF

        CALL    @EXIT           ; Quitter le programme - Retour à LDOS

;---------------------------------------------------------------------
; #### Mes fonctions!
;---------------------------------------------------------------------
; Sous-routine : NumberToString
; Entrée : A contient le nombre (0-255)
;          HL contient l'adresse du tampon de sortie
; Sortie : [HL] contient la chaîne ASCII terminée par zéro

NumberToString:
    LD B, 0                     ; B = nombre de chiffres extraits
    LD C, A                     ; C = copie du nombre original

    ; Extraire les centaines
    LD A, C
    LD D, 0
HundredsLoop:
    CP 100
    JR C, SkipHundreds
    SUB 100
    INC D
    JR HundredsLoop
SkipHundreds:
    LD C, A                     ; C = reste après centaines
    LD A, D
    CALL AddDigitToBuffer
    INC B                       ; un chiffre extrait

    ; Extraire les dizaines
    LD A, C
    LD D, 0
TensLoop:
    CP 10
    JR C, SkipTens
    SUB 10
    INC D
    JR TensLoop
SkipTens:
    LD C, A                     ; C = unités
    LD A, D
    CALL AddDigitToBuffer
    INC B

    ; Extraire les unités
    LD A, C
    CALL AddDigitToBuffer
    INC B

    ; Terminer la chaîne par CR/LF
    LD (HL), ENTER
    RET

;------------------------------------
; Sous-routine : AddDigitToBuffer
; Entrée : A contient un chiffre (0-9)
;          HL pointe vers l’endroit où écrire
; Sortie : écrit le chiffre converti en ASCII à (HL) et incrémente HL
AddDigitToBuffer:
    ADD A, '0'                  ; Convertir en ASCII
    LD (HL), A
    INC HL
    RET

;------------------------------------------------------------
; Sous-routine : NumberToHex
; Convertir un nombre en HEX.
; Entrée : DE contient un nombre
;          HL pointe vers l’endroit où écrire
; Sortie : écrit le chiffre converti en ASCII à (HL) et incrémente HL
;
NumberToHex:
   ; show "0x" as a prefix
   ld a, '0'
   LD (HL), A
   INC HL
   ld a, 'x'
   LD (HL), A
   INC HL
   ; Store the high-value
   ld  c,d
   call  OutHex8

   ; Store the low-value
   ld  c,e
   call OutHex8
   LD (HL), ENTER
   ; Return to our caller
   ret

;
; Output the hex value of the 8-bit number stored in C
;
OutHex8:
   ld  a,c
   rra
   rra
   rra
   rra
   call  Conv
   ld  a,c
Conv:
   and  $0F
   add  a,$90
   daa
   adc  a,$40
   daa
   ; Store the value.
   LD (HL), A
   INC HL
   ret

;---------------------------------------------------------------------
; FIN du programme
;---------------------------------------------------------------------
        END     MAIN
```

---

## Afficher sans OS

```
; ----------------------------------------------------
; Fichier:  Projets/afficher.asm
; Auteur:   Alain Boudreault
; Date:     2025.08.12 - Soirée des percéides
; Cible:    TRS-80 modèle 3
; ----------------------------------------------------
; Description: Afficher à l'écran sans utiliser les
;              appels système de LDOS 5.1/MOD3.
; ----------------------------------------------------

            ORG     7000H

VIDEO       EQU     3C00H
VSIZE       EQU     1024
TITRE       DEFB    'Ce menu ne sert absolument a rien', 0
LIGNE       DEFB    '---------------------------------', 0
MENU01      DEFB    '1. Manger des pommes', 0
MENU02      DEFB    '2. Activer la manivelle', 0
MENU03      DEFB    '3. Courir plus vite', 0
MENU04      DEFB    '4. Esperer mieux', 0
MENU99      DEFB    'Faites votre choix: _', 0

MAIN:
    CALL    CLEAR_SCREEN        ; Efface l'écran
    CALL    DISPLAY_MENU        ; Affiche le menu
YO:
    JP      YO                  ; Boucle infinie

; -----------------------------------------------------------------
DISPLAY_MENU:
    LD      DE, TITRE           ; Affiche le titre
    LD      B, 0                ; Ligne
    LD      C, 0                ; Colonne
    CALL    DISPLAY_ZSTRING
    INC     B                   ; Passe à la ligne suivante
    LD      DE, LIGNE           ; Affiche la ligne de séparation
    CALL    DISPLAY_ZSTRING
    INC     B                   ; Passe à la ligne suivante
    LD      DE, MENU01          ; Affiche le menu
    CALL    DISPLAY_ZSTRING
    INC     B                   ; Passe à la ligne suivante
    LD      DE, MENU02
    CALL    DISPLAY_ZSTRING
    INC     B                   ; Passe à la ligne suivante
    LD      DE, MENU03
    CALL    DISPLAY_ZSTRING
    INC     B                   ; Passe à la ligne suivante
    LD      DE, MENU04
    CALL    DISPLAY_ZSTRING
    INC     B                   ; Passe à la ligne suivante
    LD      DE, LIGNE           ; Affiche la ligne de séparation
    CALL    DISPLAY_ZSTRING
    INC     B                   ; Sauter deux lignes  - Pas possible de faire ADD B,2  ;-)
    INC     B
    LD      DE, MENU99
    CALL    DISPLAY_ZSTRING
    RET

; -----------------------------------------------------------------
; Affiche une chaîne terminée par 0 à une position (ligne, colonne)
; Entrée :
;   DE = adresse de la chaîne (à ne pas modifier)
;   B  = ligne (0 à 15)
;   C  = colonne (0 à 63)
; -----------------------------------------------------------------
; NOTE: Code généré par chatGPT, à optimiser!
; -----------------------------------------------------------------
DISPLAY_ZSTRING:
    PUSH HL
    PUSH AF
    PUSH DE           ; Sauvegarde DE pour pouvoir l’utiliser après

    ; HL = ligne * 64
    LD   A, B
    LD   H, 0
    LD   L, A         ; HL = B

    ADD  HL, HL       ; HL = B * 2
    ADD  HL, HL       ; *4
    ADD  HL, HL       ; *8
    ADD  HL, HL       ; *16
    ADD  HL, HL       ; *32
    ADD  HL, HL       ; *64

    ; HL = HL + colonne
    LD   A, C
    LD   D, 0
    LD   E, A         ; DE = colonne (temporaire)
    ADD  HL, DE       ; HL = offset dans mémoire vidéo

    ; Ajouter base mémoire vidéo
    LD   DE, 0x3C00
    ADD  HL, DE       ; HL = adresse écran finale

    POP  DE           ; Restaure DE = adresse de la chaîne

.NEXT_CHAR:
    LD   A, (DE)      ; Lire caractère
    CP   0
    JR   Z, .DONE

    LD   (HL), A      ; Écrire caractère en mémoire vidéo
    INC  DE           ; Caractère suivant dans la chaîne
    INC  HL           ; Adresse suivante à l'écran
    JR   .NEXT_CHAR

.DONE:
    POP  AF
    POP  HL
    RET

; -----------------------------------------------------------------
; Efface l'écran texte (64 colonnes × 16 lignes)
; en remplissant la mémoire vidéo avec des espaces (ASCII 32)
; Mémoire vidéo de 1024 octets à partir de 0x3C00
; -----------------------------------------------------------------
CLEAR_SCREEN:
        PUSH    DE
        PUSH    HL
        PUSH    AF
        LD      DE,VSIZE
        LD      HL, VIDEO
LOOP_CLS:
        LD      A,' '
        LD      (HL),A
        INC     HL
        DEC     DE
        LD      A,D
        OR      E
        JP      NZ,LOOP_CLS
        POP     AF
        POP     HL
        POP     DE
    RET

; -----------------------------------------------------

        END     MAIN
```

---

Documentation de LDOS

[LDOS_v5.1_for_Model_I_and_III_19xx_Tandy](https://ve2cuy.com/blog/wp-content/uploads/2025/08/LDOS_v5.1_for_Model_I_and_III_19xx_Tandy.pdf)[Télécharger](https://ve2cuy.com/blog/wp-content/uploads/2025/08/LDOS_v5.1_for_Model_I_and_III_19xx_Tandy.pdf)
