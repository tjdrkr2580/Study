
> 개념보다 오늘 해결한 문제를 적어보았습니다.


React Hook Form을 사용해서 Input 컴포넌트를 빼서 여러 곳에서 사용할 수 있도록 하려했으나, register, error 문구 처리에 대해서 조금 어려움을 겪었고 이를 해결함.


```jsx
const { register, formState: errors, handleSubmit } = useForm();


<Input
placeholder="이메일을 입력하세요."
register={register}
name="username"
type="text"
label="이메일 주소"
/>

```

이런 식으로 register을 props로 넘겨주면 Input 컴포넌트 안에서 이용이 가능하다.

```jsx
const Input = ({ register, placeholder, type, name, error, label }) => {
const [passwordVisible, setPasswordVisible] = useState(false);
const onVisible = (e) => {
e.stopPropagation();
setPasswordVisible(!passwordVisible);
};
return (
<>
{type !== "password" ? (
<InputLayout>
<Label>{label}</Label>
<InputWrapper>
<InputCustom
placeholder={placeholder}
type={type}
{...register(name, {
required: "값을 입력해주세요.",
})}
/>
{error && <ErrorMessage>{error}</ErrorMessage>}
</InputWrapper>
</InputLayout>
) : (
<InputLayout>
<Label>{label}</Label>
<InputWrapper>
<InputCustom
placeholder={placeholder}
type={passwordVisible ? "text" : type}
{...register(name, {
required: "값을 입력해주세요.",
})}
/>
<IconsWrapper onClick={onVisible}>
{passwordVisible === true ? (
<AiFillEyeInvisible size={20} />
) : (
<AiFillEye size={20} />
)}
</IconsWrapper>
{error && <ErrorMessage>{error}</ErrorMessage>}
</InputWrapper>
</InputLayout>
)}
</>
);
};
```

