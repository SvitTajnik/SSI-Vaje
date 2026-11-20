# Vaja 5: Naprednejše Bash skripte (funkcije, vnos podatkov, izračuni)

V tej vaji bomo uporabili funkcije, delo z uporabniškim vnosom, izračuni ter rekurzijo v Bash skriptah. Na koncu so dodane še dodatne naloge za izziv.

---

## **Vaja 1: Prikaz časa, datuma in prijavljenih uporabnikov**

1. Ustvarite skripto `system_info.sh`, ki izpiše:
   - trenutni datum
   - trenutni čas
   - aktivne prijavljene uporabnike

Primer skripte:

```bash
#!/bin/bash

echo "Trenutni datum: $(date +%Y-%m-%d)"
echo "Trenutni čas: $(date +%H:%M:%S)"
echo "Prijavljeni uporabniki:"
who
```

Zaženite skripto:

```bash
chmod +x system_info.sh
./system_info.sh
```

---

## **Vaja 2: Izračun povprečja ocen**

1. Ustvarite skripto `grades.sh`, ki:
   - omogoča uporabniku vnos poljubnega števila ocen
   - zaključi vnos, ko uporabnik vnese `0`
   - izračuna povprečje vseh vnesenih ocen (razen 0)

Primer skripte:

```bash
#!/bin/bash

sum=0
count=0
grade=1

echo "Vnašaj ocene (0 za konec):"

while [ $grade -ne 0 ]
do
    read grade
    if [ $grade -ne 0 ]; then
        sum=$((sum + grade))
        count=$((count + 1))
    fi
done

if [ $count -gt 0 ]; then
    echo "Povprečna ocena: $((sum / count))"
else
    echo "Ni vnešenih ocen."
fi
```

---

## **Vaja 3: Rekurzivna funkcija – Fakulteta**

Ustvarite skripto `fakulteta.sh`, ki:

1. prebere število od uporabnika  
2. uporabi **rekurzivno funkcijo**, ki izračuna fakulteto  
3. izpiše rezultat  

Primer:

```bash
#!/bin/bash

fakulteta() {
    if [ $1 -le 1 ]; then
        echo 1
    else
        local prev=$(fakulteta $(( $1 - 1 )))
        echo $(( $1 * prev ))
    fi
}

echo "Vnesi število:"
read num

rezultat=$(fakulteta $num)
echo "Fakulteta od $num je: $rezultat"
```

---

# **Dodatne vaje**

## **Dodatna vaja 1: Najdite največje vneseno število**

1. Ustvarite skripto `max_number.sh`, ki:
   - sprejema števila, dokler uporabnik ne vnese `stop`
   - shrani največje vneseno število
   - izpiše rezultat

<details>
<summary>Rešitev</summary>

```bash
#!/bin/bash

max=""

echo "Vnašaj števila (stop za konec):"

while true
do
    read num
    if [ "$num" = "stop" ]; then
        break
    fi
    if [ -z "$max" ] || [ "$num" -gt "$max" ]; then
        max=$num
    fi
done

if [ -n "$max" ]; then
    echo "Največje število: $max"
else
    echo "Ni vnešenih števil."
fi
```

</details>

---

## **Dodatna vaja 2: Preštejte pojavitev besede v datoteki**

Ustvarite skripto, ki:

- vpraša uporabnika za ime datoteke
- vpraša uporabnika za iskano besedo
- izpiše, kolikokrat se beseda pojavi v datoteki (grep + wc)

<details>
<summary>Rešitev</summary>

```bash
#!/bin/bash

echo "Vnesi ime datoteke:"
read file

echo "Vnesi iskano besedo:"
read word

if [ ! -f "$file" ]; then
    echo "Datoteka ne obstaja."
    exit 1
fi

count=$(grep -o -w "$word" "$file" | wc -l)
echo "Beseda '$word' se pojavi $count-krat."
```

</details>

---

## **Dodatna vaja 3: Izpis vseh datotek z ekstenzijo**

Ustvarite skripto, ki:

- prebere ekstenzijo (npr. txt)
- izpiše vse datoteke s to končnico v trenutni mapi (find)

<details>
<summary>Rešitev</summary>

```bash
#!/bin/bash

echo "Vnesi ekstenzijo (npr. txt):"
read ext

find . -maxdepth 1 -type f -name "*.$ext"
```

</details>

---

## **Dodatna vaja 4: Mini kalkulator**

Ustvarite skripto z menijem:

```
1 – seštevanje
2 – odštevanje
3 – množenje
4 – deljenje
0 – izhod
```

Skripta naj omogoča več izvajanj, dokler uporabnik ne izbere `0`.

<details>
<summary>Rešitev</summary>

```bash
#!/bin/bash

while true
do
    echo "1 – seštevanje"
    echo "2 – odštevanje"
    echo "3 – množenje"
    echo "4 – deljenje"
    echo "0 – izhod"
    read izbira

    if [ "$izbira" = "0" ]; then
        echo "Izhod."
        break
    fi

    echo "Vnesi prvo število:"
    read a
    echo "Vnesi drugo število:"
    read b

    case $izbira in
        1) echo "Rezultat: $((a + b))" ;;
        2) echo "Rezultat: $((a - b))" ;;
        3) echo "Rezultat: $((a * b))" ;;
        4)
            if [ "$b" -eq 0 ]; then
                echo "Deljenje z 0 ni dovoljeno."
            else
                echo "Rezultat: $((a / b))"
            fi
            ;;
        *) echo "Neveljavna izbira." ;;
    esac
done
```

</details>

---