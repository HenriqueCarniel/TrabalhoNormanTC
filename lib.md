// Limpa variável
operation clear(A){
    1: if zero A then goto 0 else goto 2
    2: do dec A goto 1
}

// A += B (soma destrutiva)
operation sumDest(A,B){
    1: if zero B then goto 0 else goto 2
    2: do dec B goto 3
    3: do inc A goto 1
}

// A = B (atribuição destrutiva)
operation movDest(A,B){
    1: do clear(A) goto 2
    2: do sumDest(A,B) goto 0
}

// A += B usando C
operation sum(A,B,C){
    1: if zero B then goto 5 else goto 2
    2: do dec  B goto 3
    3: do inc  A goto 4
    4: do inc  C goto 1
    5: do sumDest(B,C) goto 0
}

// A:=A-B usando C (subtração conservativa) 
operation sub(A,B,C){ 
    1: do atribui(C,B) goto 2 
    2: if zero C then goto 0 else goto 3 
    3: do dec C goto 4 
    4: do inc B goto 5 
    5: do dec A goto 2 
}

// A = B usando C (atribuição não-destrutiva)
operation mov(A,B,C){
  1: do clear(A) goto 2
  2: do sum(A,B,C) goto 0
}

// A = 2A usando C
operation mult2(A,C){
    1: do movDest(C,A) goto 2
    2: if zero C then goto 0 else goto 3
    3: do dec C goto 4
    4: do inc A goto 5
    5: do inc A goto 2 
}

// A = A x B usando C e D
operation mult(A,B,C,D){
    1: do movDest(C,A) goto 2
    2: if zero C then goto 0 else goto 3
    3: do sum(A,B,D) goto 4
    4: do dec C goto 2
}

// A = 2^B usando C e D
operation pow2(A,B,C,D){
    1: do inc A goto 2
    2: do mov(D,B,C) goto 3
    3: if zero D then goto 0 else goto 4
    4: do mult2(A,C) goto 5
    5: do dec D goto 3
}

// A % 2 =? 0
test divBy2(A,C){
    1: do movDest(C,A) goto 2
    2: if zero C then goto true else goto 3
    3: do dec C goto 4
    4: do inc A goto 5
    5: if zero C then goto false else goto 6
    6: do dec C goto 7
    7: do inc A goto 2
}

// A = A // 2 usando C
operation div2(A,C){
    1: do movDest(C,A) goto 2
    2: if zero C then goto 0 else goto 3
    3: do dec C goto 4
    4: if zero C then goto 0 else goto 5
    5: do dec C goto 6
    6: do inc A goto 2
}

// A ?= 1
test one(A){
    1: if zero A then goto false else goto 2
    2: do dec A goto 3
    3: if zero A then goto 4 else goto 5
    4: do inc A goto true
    5: do inc A goto false
}

// Codificação 2^A(2B+1)
operation cod(A,B){
    1: do pow2(M,A,C,D) goto 2
    2: do mov(N,B,C) goto 3
    3: do mult2(N,C) goto 4
    4: do inc N goto 5
    5: do inc C goto 6
    6: do mult(C,M,D,E) goto 7
    7: do mult(C,N,D,E) goto 0
}

// Decodificação
operation decod(C){
    1: do mov(D,C,E) goto 2
    2: if zero D then goto 0 else goto 3
    3: if divBy2(D,E) then goto 4 else goto 6
    4: do div2(D,E) goto 5
    5: do inc A goto 3
    6: do dec D goto 7
    7: do div2(D,E) goto 8
    8: do movDest(B,D) goto 0
}