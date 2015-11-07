# Metin2 Questfunktion: `pet.is_mine()`

Diese Erweiterung fügt eine neue Lua-Questfunktion hinzu, um festzustellen, ob ein Pet dem Spieler gehört, der aktuell mit dem Pet interagiert.

📅 Veröffentlichung: [07.11.2015, 11:17](https://www.elitepvpers.com/forum/metin2-pserver-guides-strategies/3913072-release-c-pet-is_mine-questfunction.html)

---

## 🧠 Funktion

```cpp
bool pet.is_mine()
```

Gibt `true` zurück, wenn das angeklickte Pet dem aktuellen Spieler gehört.

---

## 📌 Verwendung in Quests

```lua
when 34001.click with pet.is_mine() begin
    -- Belohnung oder weitere Logik
end
```

---

## 🧩 Implementierung in `questlua_pet.cpp`

### Hinzufügen:

```cpp
int pet_is_mine(lua_State* L)
{
    CQuestManager& q = CQuestManager::instance();
    LPCHARACTER mch = q.GetCurrentCharacterPtr();
    LPCHARACTER tch = q.GetCurrentNPCCharacterPtr();
    CPetSystem* petSystem = mch->GetPetSystem();
    CPetActor* petActor = petSystem->GetByVID(tch->GetVID());

    lua_pushboolean(L, tch && tch->IsPet() && petActor && petActor->GetOwner() == mch);
    return 1;
}
```

### Registrierung:

In der Initialisierungsfunktion des Pet-Namespaces ergänzen:

```cpp
{"is_mine", pet_is_mine},
```

---

## 🗂️ Quelle

- [elitepvpers-Release von 2015](https://www.elitepvpers.com/forum/metin2-pserver-guides-strategies/3913072-release-c-pet-is_mine-questfunction.html)
