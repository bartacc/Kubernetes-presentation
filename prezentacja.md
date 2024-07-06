1. Spis treści
2. Książka
3. Czym jest kontener? https://kubernetes.io/docs/concepts/overview/
4. Problem - zarządzanie zbiorem kontenerów (mikroserwisów)
    - Restartowanie konterów w przypadku crasha / deadlocka
    - Przeniesienie kontenera na inną maszynę (VM) w przypadku awarii sprzętu
    - Skalowanie zależne od natężenia ruchu (duplikacja kontenerów)
    - Konfiguracja sieci
    - Dostęp do konfiguracji całego klastra w jednym miejscu
    - Rollout/rollback wszystkich kontenerów
5. Historia
   - 2004: Borg - Rozwiązanie closed source od Google'a do zarządzania klastrami
   - 2015: Kubernetes 1.0 - Stworzone przez Google, od początku open source
           Powstanie CNCF (Cloud Native Computing Foundation)
   - 2016: Helm - package manager dla Kubernetes
   - 2017: Wsparcie od VMWare, Docker, Microsoft Azure, AWS
6. Filozofia Kubernetes - deklaratywny kod specyfikuje pożądany stan systemu, a kubernetes robi wszystko, żeby go osiągnąć
7. Podsumowanie komponentów (https://kubernetes.io/docs/concepts/overview/components/)
8. Pod - zbiór kontenerów i woluminów (volumes, np. baza danych) działających na jednej maszynie (node)
   - Najmniejsza jednostka w Kubernetes
   - Każdy pod ma osobne IP
   - Każdy kontener wewnątrz poda jest w osobnej cgroupie, ale dzieli niektóre namespace'y z innymi
   - Przykład 5.1
   - Health checks (liveness probe - restartuje kontener po kilku porażkach, readiness probe - wyłącza spod z load balancera po kilku porażkach)
   - Zarządzanie zasobami (minimalne - gwarantowane, maksymalne - nie do przekroczenia)
   - Przykład 5.6
9.  Pod Demo
10. ReplicaSet
    - Tworzy kopie danego poda   
11. ReplicaSet demo
12. Deployment 
    - Rolling update
    - Podgląd historii i łatwy powrót do poprzednich wersji
13. Deployment demo
14. Service discovery
    - Problem: które procesy słuchają na jakich adresach pod jakimi portami?
    - Rozwiązanie:
      - Stwórz serwis, w którym każdy pod automatycznie dostaje label identyfikujący serwis (np. `app=my-app`)
      - Żeby dostać się do konkretnego serwisu, wystarczy znać jego port i dostać się do dowolnego node'a, a ten przekieruje nas do poda (być może na innym node), gdzie znajduje się dany serwis. To wykorzystuje kube-proxy oraz wewnętrzny serwis dns.
      - Alternatywnie osobny load balancer (tylko w chmurze)
15. Service discovery demo
16. DaemonSet
    - Umieszcza po jednej kopii danego poda na każdym node
17. Jobs
    - Pojedyncze wykonanie
    - Wykonanie zadań z kolejki i wyjście
    - CronJob
18. Podsumowanie komponentów (https://kubernetes.io/docs/concepts/overview/components/)
19. Co ma Kubernetes czego nie ma Docker Swarm?
    - Wsparcie dla innych typów kontenerów niż docker (Containerd, CRI-O, ...)
    - Automatyczne skalowanie (np. na podstawie zużycia procesora)
    - Automatyczne zarządzanie sekretami
    - RBAC (Role-based access control)
    - Automatyczny self-healing
    - Duże wsparcie społeczności (pluginy)
    - Dostępne jako gotowy serwis u popularnych dostawców chmurowych (GCP, AWS, Azure)
    - Wbudowany monitoring i dashboard
20. Zalety Docker Swarm
    - Prostszy, szybszy do nauki
    - Wbudowany w docker engine, zintegrowany z narzędziami dockerowymi
    - Automatyczny load balancing
    - Idealny do mniejszych projektów
21.  StatefulSet z przykładem MongoDB?   




